#!/usr/bin/env/groovy

// Make sure this build gets a unique label
String label = "buildpod.${env.JOB_NAME}.${env.BUILD_NUMBER}".replaceAll(/[^\w-]/, '_')

// Sets special branch locations
String officialMaster = 'git@github.com:Exabel/api.git:master'

// Build levels
BUILD_NONE = 0    // Build nothing -- assuming no code changes
BUILD_PUBLISH = 1 // Build only what's needed to publish maven artifacts
BUILD_ALL = 2     // Build, test and publish everything

// List of files that do not contain any code
Collection<String> fileRegexNoCode = [/common/, /.*[.]md/, /.*[.]png/, /.*[.]dot/, /.*[.]svg/]

// Returns a list of changed files since the previous successful build.
Collection<String> getChangedFilesList() {
  Collection<String> files = new HashSet()
  def build = currentBuild
  while (build != null && build.result != 'SUCCESS') {
    files.addAll(
      build.getChangeSets().collectMany { it.collectMany { it.getAffectedFiles().collect { it.getPath() }}})
    build = build.previousBuild
  }
  if (build != null) {
    echo "Previous successful build: $build.displayName"
    return files
  }

  echo 'No previous successful builds'
  if (env.CHANGE_ID) {
    // This is a PR, return the files as reported by GitHub.
    return pullRequest.files.collect { it.filename }
  } else {
    // This is a pure branch build, return something that will build everything.
    return ['__first_build__']
  }
}

podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: jnlp
      env:
      - name: GKE_NODE_NAME
        valueFrom:
          fieldRef:
            fieldPath: spec.nodeName
""",
  containers: [
    containerTemplate(
      name: 'mvn', image: 'eu.gcr.io/jenkins-exabel/maven-build:v16',
      ttyEnabled: true, command: 'cat',

      resourceRequestCpu: '2.5', resourceLimitCpu: '3.5',
      resourceRequestMemory: '6Gi', resourceLimitMemory: '8Gi',
    ),
    containerTemplate(
      name: 'prototool', image: 'uber/prototool:1.10.0',
      ttyEnabled: true, command: 'cat',

      resourceRequestCpu: '0.5', resourceLimitCpu: '3.5',
      resourceRequestMemory: '200Mi', resourceLimitMemory: '2Gi',
    ),
    containerTemplate(
      name: 'jnlp', image: 'eu.gcr.io/jenkins-exabel/jenkins-k8s-slave:v6',
      args: '${computer.jnlpmac} ${computer.name}',
      resourceRequestCpu: '0.1', resourceLimitCpu: '3.0',
      resourceRequestMemory: '150Mi', resourceLimitMemory: '4Gi'
    ),
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
    hostPathVolume(mountPath: '/root/.m2', hostPath: '/home/mvn/shared_across_containers/.m2'),
  ]) {

  node(label) {
    try {
      stage('Checkout') {
        echo "*** Running on node ${env.GKE_NODE_NAME} ***"
        checkout([
                $class           : 'GitSCM',
                branches         : scm.branches,
                extensions       : scm.extensions + [
                        [$class: 'SubmoduleOption', parentCredentials: true, recursiveSubmodules: true],
                        [$class: 'LocalBranch', localBranch: BRANCH_NAME],
                ],
                userRemoteConfigs: scm.userRemoteConfigs
        ])
      }

      stage('Get git environment') {
        gitRemote = sh(returnStdout: true, script: 'git config remote.origin.url').trim()
        gitBranch = BRANCH_NAME
        gitCommit = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
        tagName = (gitBranch + '.' + gitCommit).replaceAll(/[^\w-\.]/, '_')
        ON_MASTER = (gitRemote + ':' + gitBranch == officialMaster)

        changed = getChangedFilesList()
        echo 'Changed files: ' + changed

        if (changed.every { f -> fileRegexNoCode.any { f ==~ it } }) {
          echo 'No code changes -- do not build anything.'
          buildLevel = BUILD_NONE
        } else {
          echo 'Full build.'
          buildLevel = BUILD_ALL
        }
      }

      if (buildLevel >= BUILD_PUBLISH) {
        container('prototool') {
          stage('Prototool linting and formatting') {
            sh 'prototool lint'
            sh 'prototool format -l'
          }
          stage('Prototool breaking change') {
            sh 'prototool break check'
          }
        }
        container('mvn') {
          try {
            stage('Maven build, test, package and verify') {
              String mvnProfile = buildLevel == BUILD_ALL ? 'jenkins' : 'jenkins-simple'
              sh 'mvn -B install -P' + mvnProfile

              step([$class: 'WarningsPublisher', consoleParsers: [[parserName: 'Maven'], [parserName: 'Java Compiler (javac)']]])
              step([$class: 'AnalysisPublisher'])

              // Stop build if failed by a publisher
              if (currentBuild.result == 'FAILURE') {
                error 'Failed due to static code analysis';
              }
            }
            stage('Test endpoint definitions') {
              Collection<String> serviceFiles = findFiles(glob: 'exabel/*/*/*-api.yaml').collect { it.path }
              for (String serviceFile in serviceFiles) {
                String protoDescriptor = serviceFile[0..serviceFile.lastIndexOf('/')] +
                  "target/generated-resources/protobuf/descriptor-sets/descriptor.pb"
                sh "common/endpoints/validate_config.sh ${serviceFile} ${protoDescriptor}"
              }
            }
          } finally {
            if (buildLevel >= BUILD_ALL) {
              // TODO(nordli): Enable when tests are actually running.
              // stage('Publish test results') {
              //   junit '**/target/surefire-reports/*.xml'
              // }
            }
          }
        }

        if (ON_MASTER && buildLevel >= BUILD_PUBLISH) {
          stage('Deploy maven artifacts') {
            container('mvn') {
              String output = sh(returnStdout: true, script: 'mvn -B deploy -Dmaven.install.skip').trim()
              echo output
              // Send Slack notifications by searching for "Uploading*.pom". Sample:
              // Uploading to exabel-cloud-repository-release: gs://jenkins-exabel#exabel-maven-repo/release/com/exabel/api/data/1.0.8/data-1.0.8.pom
              for (String line in output.split('\n').findAll { it ==~ '.*Uploading.*\\.pom'}) {
                Collection<String> parts = line.substring(line.indexOf("/release/") + 9).split('/').dropRight(1)
                String mvn = parts.dropRight(2).join('.') + ":" + parts.takeRight(2).join(':')
                notifySlack('#40b59b', "${mvn} is published.")
              }
            }
          }
          stage('Deploy endpoints definitions') {
            Collection<String> serviceFiles = findFiles(glob: 'exabel/*/*/*-api.yaml').collect { it.path }
            for (String serviceFile in serviceFiles) {
              String protoDescriptor = serviceFile[0..serviceFile.lastIndexOf('/')] +
                "target/generated-resources/protobuf/descriptor-sets/descriptor.pb"
              if (sh(returnStatus: true, script: "common/endpoints/changed.sh ${serviceFile} ${protoDescriptor}") != 0) {
                sh "common/endpoints/deploy.sh ${serviceFile} ${protoDescriptor}"
                String version = sh(returnStdout: true, script: "common/endpoints/latest_id.sh ${serviceFile}")
                notifySlack('#40b59b', "${version} is deployed.")
              }
            }
          }
        }
      }

      if (!ON_MASTER) {
        notifySlack('#40b59b', 'SUCCESS')
      }
    } catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
      notifySlack('#ffaa00', 'CANCELLED')
      throw e
    } catch (Exception e) {
      notifySlack('#f51767', 'FAILURE')
      throw e
    }
  }
}

String getSlackUser() {
  echo "Finding slack user for: ${env.CHANGE_AUTHOR}"
  echo "Slack user mapping: ${env.SLACK_USER_MAPPING}"
  map = env.SLACK_USER_MAPPING.split(';').collectEntries { entry ->
    def pair = entry.split(':')
    [(pair.first()): pair.last()]
  }
  return map.get(env.CHANGE_AUTHOR)
}

String getChannel() {
  if (ON_MASTER) {
    return '#jenkins-ci'
  }
  return "@${getSlackUser()}"
}

def notifySlack(String color, String message) {
  pull_request_info = env.CHANGE_ID ? "\nPull request: https://github.com/Exabel/api/pull/${env.CHANGE_ID}" : ''
  slackSend channel: getChannel(),
          color: color,
          message: "${message}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}${pull_request_info}"
}
