
## How to Release

* Prepare release PR:
    * Pull the changes in the helm-charts sub module folder to get the latest main.
      * `cd helm-charts && git pull origin main`.
    * Update any changing versions in [release.json](../release.json).
    * Ensure that [the release notes](./RELEASE_NOTES.md) are up to date for this release.
    * Run `python scripts/ci/update_release.py` to update the relevant yaml manifests.
    * Copy `CRD`s to Helm Chart
      - `cp config/crd/bases/mongodbcommunity.mongodb.com_mongodbcommunity.yaml helm-charts/charts/community-operator-crds/templates/mongodbcommunity.mongodb.com_mongodbcommunity.yaml`
      - commit changes to the [helm-charts submodule](https://github.com/mongodb/helm-charts) and create a PR against it ([similar to this one](https://github.com/mongodb/helm-charts/pull/163)).
      - do not merge helm-charts PR until release PR is merged and the images are pushed to quay.io.
      - do not commit the submodule change in the release pr of the community repository.
    * Commit all changes (except for the submodule change)
    * Create a PR with the title `Release MongoDB Kubernetes Operator v<operator-version>` (the title must match this pattern).
    * Wait for the tests to pass and merge the PR.
      * Upon approval, all new images for this release will be built and released, and a GitHub release draft will be created.
        * Dockerfiles for mongodb-kubernetes-operator and mongodb-agent will be uploaded to S3 to be used by daily rebuild process in the enterprise repo.
      * Review and publish the new GitHub release draft, that was prepared
    * Merge helm-charts PR and update submodule to the latest commit on `main` branch.
    * Create a new PR with only bump to the helm-chart submodule, similar to [this](https://github.com/mongodb/mongodb-kubernetes-operator/pull/1210). The commit here should match the master commit in the `helm-charts` repository.
    * Add the new released operator version to the enterprise [release.json](https://github.com/10gen/ops-manager-kubernetes/blob/master/release.json#L74) file.
