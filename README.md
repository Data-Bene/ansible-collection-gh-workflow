# ansible-collection-gh-workflow for github worflow of Ansible Collection repositories

GitHub reusable workflow for running expected regression tests from an Ansible
Collections.

Note: GITHUB Reusable workflows are currently in beta and subject to change.


## Usage

To use the action add the following step(s) to your workflow file (e.g.
`.github/workflows/ansible-test.yml`). It's also advisable to set up distinct
workflows to reduce the number of jobs per workflow (e.g.
`.github/workflows/ansible-test-units.yml` for unit testing, ...):

```yaml
jobs:
  sanity:
    uses: c2main/ansible-collection-gh-workflow/.github/workflows/workflow.yml@devel
    with:
      fail-fast: false
      testing-type: sanity

  units:
    uses: c2main/ansible-collection-gh-workflow/.github/workflows/workflow.yml@devel
    with:
      fail-fast: false
      testing-type: units

  pg-14:
    uses: c2main/ansible-collection-gh-workflow/.github/workflows/workflow.yml@devel
    with:
      pre-test-cmd: "sed -i 's/^pg_version:.*/pg_version: \"14\"/g' ./tests/integration/targets/setup_postgresql_db/defaults/main.yml"
      test-python: false
      testing-type: integration
```

> **Pro tip**: instead of using branch pointers, like `main`, pin
versions of Actions that you use to tagged versions or SHA-1 commit
identifiers. This will make your workflows more secure and better
reproducible, saving you from sudden and unpleasant surprises.

## inputs

Parameters are used to customized the reusable workflow which in turn may just
customize actions used by the reusable workflow:

* `fail-fast`: (type: boolean) GitHubAction parameter for jobs strategy (default: true, required: false)
* `pre-test-cmd`: (type: string) Parameter for action paths-filter (default:, required: false)
* `runs-on`: (type: string) docker image to run GHA (default: 'ubuntu-latest', required: false)
* `test-docker`: (type: boolean) Test on all docker images (default: true, required: false)
* `test-python`: (type: boolean) Test on all python versions (target) (default: true, required: false)
* `testing-type`: (type: string) Parameter for ansible-test-gh-action (default: integration, required: true)

When set the inputs replace the default values for the matrix job builder, inputs MUST be JSON:

* `ansible-core-version`: Array of ansible versions (required: false)
* `docker-exclude`: Array of matrix excludes for docker (required: false)
* `docker-image`: Array of docker images (required: false)
* `docker-include`: Array of matrix includes for docker (required: false)
* `python-version`: Array of python versions (controler) (required: false)
* `target-python-exclude`: Array of excludes target python versions (required: false)
* `target-python-version`: Array of target python versions (managed) (required: false)

# Developed and Sponsored by

[Data Bene](https://data-bene.io)
