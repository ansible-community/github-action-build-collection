<!--
Copyright (c) Ansible Project
GNU General Public License v3.0+ (see LICENSES/GPL-3.0-or-later.txt or https://www.gnu.org/licenses/gpl-3.0.txt)
SPDX-License-Identifier: GPL-3.0-or-later
-->

# GitHub Action for building Ansible collections

[![Linting](https://github.com/ansible-community/github-action-build-collection/actions/workflows/linting.yml/badge.svg)](https://github.com/ansible-community/github-action-build-collection/actions/workflows/linting.yml)
[![REUSE](https://github.com/ansible-community/github-action-build-collection/actions/workflows/reuse.yml/badge.svg)](https://github.com/ansible-community/github-action-build-collection/actions/workflows/reuse.yml)

A composite GitHub Action that allows to build an Ansible collection artifact in GitHub Actions CI/CD workflows.

This action is covered by the [Ansible Code of Conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html).

## Usage

To use the action, add the following step to your workflow file (for example `.github/workflows/build-collection.yml`):

```yaml
- name: Check out your collection repository
  uses: actions/checkout@v4

- name: Build collection
  id: build-collection
  uses: ansible-community/github-action-build-collection@main

- name: Upload built collection as artifact
  uses: actions/upload-artifact@v4
  with:
    name: my-collection
    path: ${{ steps.build-collection.outputs.artifact-filename }}
```

## Options

The follow options can be provided to this GitHub Action.

### `python-version`

The Python version to use for running ansible-core.

**(DEFAULT: `3.12`)**

### `ansible-core-version`

The branch of tag name of ansible-core to install.
This is assumed to exist in https://github.com/ansible/ansible.

**(DEFAULT: `stable-2.16`)**

### `subdirectory`

The subdirectory in which the collection's sources can be found. Must contain the `galaxy.yml` file.

**(DEFAULT: `.`)**

### `collection-requirements-path`

If provided, the collection's requirements will be written as a Galaxy `requirements.yml` file to this path.

## Outputs

The GitHub Action provides the following outputs that can be used in further workflow steps.

### `collection-full-name`

The collection's full name (namespace.name).

### `collection-name`

The collection's name. For example `ansible.posix`'s name is `posix`.

### `collection-namespace`

The collection's namespace. For example `ansible.posix`'s namespace is `ansible`.

### `collection-version`

The collection's version. Will be set to `0.0.1` if no `version` field is present in `galaxy.yml`.

### `artifact-filename`

Filename of the built collection artifact.

### `artifact-path`

Path to the built collection artifact.

## Bundled shared workflow

This GitHub Action bundles a shared workflow, [build-collection.yaml](https://github.com/ansible-community/github-action-build-collection/blob/main/.github/workflows/build-collection.yml), which allows you to build a collection, and upload the built collection artifact together with its `requirements.yml` file as a GitHub artifact.

```yaml
jobs:
  build-collection:
    name: Build collection artifact
    permissions:
      contents: read
    uses: ansible-community/github-action-build-collection/.github/workflows/build-collection.yml@main
```

Please check out the workflow for the options it supports, and for which outputs it provides.

## License

This action is primarily licensed and distributed as a whole under the GNU General Public License v3.0 or later.

See [LICENSES/GPL-3.0-or-later.txt](https://github.com/ansible-community/github-action-build-collection/blob/main/COPYING) for the full text.

All files have a machine readable `SDPX-License-Identifier:` comment denoting its respective license(s) or an equivalent entry in an accompanying `.license` file. This conforms to the [REUSE specification](https://reuse.software/spec/).
