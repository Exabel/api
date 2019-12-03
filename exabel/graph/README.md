This folder contains the external graph API. This API exposes the graph data to external users.
With this API it is possible to create and modify entities and relations, define signals and add
time series.

# Resources

The API contains the following resources:

* Entity types
* Entities
* Relationship types
* Relationships
* Signals
* Time series.

## Versioning

When modifying any APIs, the following steps needs to be followed:

1. Start a new development version:
`mvn versions:set -DnextSnapshot=true`. This will change the version of this module to
`X.Y.Z-SNAPSHOT`. (If you want a specific version, you can use
`mvn versions:set -DnewVersion=X.Y.Z-SNAPSHOT` instead.)

2. Update the dependencies (to `com.exabel.api:graph`) of the consuming project(s).

3. Perform the changes in this project, pushing to your local Maven repository: `mvn clean install`.

4. Test the changes in your other project. Repeat from step 3 if necessary.

5. When the changes are finished, finalize the version by running
`mvn versions:set -DremoveSnapshot=true` and create a PR.

6. When the PR in step 5 is completed, Jenkins will build and publish the new artifacts.

7. When step 6 is completed, update the dependencies of the consuming project(s) again and create
a new PR.

# Documentation

## Prerequisites

* Python 3
  ```
  pyenv install
  ```
* Sphinx with theme `sphinx_rtd_theme`: https://www.sphinx-doc.org/en/master/usage/installation.html
  ```
  pip3 install -U sphinx
  pip3 install sphinx_rtd_theme
  ```

## Building

Produce HTML output in the folder 'target/html':
```
make html
```
