# Helm charts for GateNLP applications

This repository holds Helm charts designed to install open-source GATE applications in a Kubernetes cluster.  The charts in this repository are deployed to the GATE Helm repository and can be installed using standard Helm commands.  To register the GATE repository with your Helm client run:

```
helm repo add gate https://repo.gate.ac.uk/repository/charts
helm repo update
```

With the repository registered, you can then install any chart using the normal

```
helm upgrade --install {release-name} gate/{chart-name} --values {override-values.yaml} --namespace {namespace}
```

The basic command above will install the latest available version of the selected chart, or you can specify a specific version or series using the `--version` option.  Helm chart version numbers are required to follow [semantic versioning](https://semver.org) rules so the version number of the _chart_ will generally be different from the version number of the _application_ that the chart installs.  See the README file in the relevant chart directory in this repository for details of exactly which app version is installed by which chart version.  Semver rules require that the major version number of the chart must change with any backwards-incompatible change to the chart templates, even if the new templates are still installing the same version of the underlying software.


## Licensing

The charts in this repository are licensed under the Apache Licence version 2.0, however the software that they install may be under one or more different and more restrictive licences.  For example the `gate-teamware` chart installs [GATE Teamware](https://github.com/GateNLP/gate-teamware), which is released under the AGPL, and [PostgreSQL](https://www.postgresql.org), which is released under its own BSD-like licence.  It is your responsibility to ensure that your use of any of these charts complies with all applicable licences.

## Information for chart developers

All updates to charts in this repository must be done via pull requests.  To modify a chart make a new branch off the current `main`, push your changes to that branch, and start a pull request.  The following automatic checks are performed on each pull request, and must all pass before the PR is merged:

- if the PR makes _any_ changes within a given chart directory, then the PR _must_ update the `version` number of that chart in the relevant `Chart.yaml`.  PRs that change a chart but do not bump its version number will be rejected.
- if the chart declares any `dependencies` in its `Chart.yaml` then the corresponding `Chart.lock` _must_ exist - you can create this using `helm dependency update`
- all dependencies in the `Chart.lock` _must_ resolve - if a dependency has been removed from its remote repository you will have to edit `Chart.yaml` if necessary and run `helm dependency update` to re-generate the lock file

When the PR is merged to `main` all modified charts will be automatically deployed to `https://repo.gate.ac.uk/repository/charts`
