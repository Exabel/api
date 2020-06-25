This folder contains the external Exabel Data API. This API exposes the graph and time series data to external users.
With this API it is possible to create and modify entities and relations, define signals and add time series.

# Resources

The Data API contains the following resources:

* Entity types
* Entities
* Relationship types
* Relationships
* Signals
* Time series

## Versioning

When modifying any APIs, the following steps needs to be followed:

1. Start a new development version:
`mvn versions:set -DnextSnapshot=true`. This will change the version of this module to
`X.Y.Z-SNAPSHOT`. (If you want a specific version, you can use
`mvn versions:set -DnewVersion=X.Y.Z-SNAPSHOT` instead.)

2. Update the dependencies (to `com.exabel.api:data`) of the consuming project(s).

3. Perform the changes in this project, pushing to your local Maven repository: `mvn clean install`.

4. Test the changes in your other project. Repeat from step 3 if necessary.

5. When the changes are finished, finalize the version by running
`mvn versions:set -DremoveSnapshot=true` and create a PR.

6. When the PR in step 5 is completed, Jenkins will build and publish the new artifacts.

7. When step 6 is completed, update the dependencies of the consuming project(s) again and create
a new PR.

# Documentation

[Original design documentation](https://docs.google.com/document/d/1_qogmUdmApPPHqzPgY_jhs0vvpqyMZBOQs_Dm5mZdL0/edit)

## Prerequisites

* Python 3
  ```
  pyenv install
  ```
* Sphinx with theme `sphinx_rtd_theme`: https://www.sphinx-doc.org/en/master/usage/installation.html
  ```
  pip3 install -U sphinx
  pip3 install sphinx_rtd_theme sphinxcontrib-httpexample
  ```
* Node.js, e.g. install with `nvm`: https://github.com/creationix/nvm#install-script
  ```
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
  nvm install
  ```
* Aerobatic CLI and account for Exabel: https://dashboard.aerobatic.com/account/exabel/
  * Get invite from grotmol@exabel.com or wergeland@exabel.com
  ```
  npm install aerobatic-cli -g
  aero login
  ```

## Building

Produce HTML output in the folder 'target/html':
```
make html
```


## Deploy

### Documentation

To production: https://exabel-api-doc.aerobaticapp.com/
```
nvm use
aero deploy
```

### Endpoints

```
gcloud endpoints services deploy target/generated-resources/protobuf/descriptor-sets/descriptor.pb data-api.yaml
```

The projects that run implementations of this API must have "Service Consumer" and "Service
Controller" permissions for this API.

API overview in Cloud Console: https://console.cloud.google.com/endpoints/api/data.api.exabel.com/overview?project=exabel-api&organizationId=935607507718

## Auth0 integration

Aerobatic uses a separate "regular web application" in Auth0 (called "Documentation"),
and the login page hosted by Auth0. Client id, client secret, and tenant is set as
environment properties Aerobatic: https://dashboard.aerobatic.com/exabel/exabel-api-doc/envvars
