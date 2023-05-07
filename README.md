<img align="right" width="22%" src="docs/molecule-logo.png" alt="molecule logo"/>

# setup-molecule

[![Test](https://github.com/bec-galaxy/setup-molecule/actions/workflows/test.yml/badge.svg)](https://github.com/bec-galaxy/setup-molecule/actions/workflows/test.yml)

This action provides the following functionality for GitHub Actions users:

- Installing Python.
- Installing the latest version of Molecule with Docker.

## Basic usage

See [action.yml](action.yml)

**Molecule**
```yaml
steps:
  - name: Checkout the codebase
    uses: actions/checkout@v3

  - name: Setup Molecule
    uses: bec-galaxy/setup-molecule@v2

  - name: Run Molecule tests
    run: molecule test
```

**Lint and Molecule**

```yaml
---
name: Molecule

"on":
  pull_request:

env:
  PY_COLORS: "1"
  ANSIBLE_FORCE_COLOR: "1"
jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the codebase
        uses: actions/checkout@v3

      - name: Setup Lint
        uses: bec-galaxy/setup-lint@v2

      - name: Run Lint tests
        run: ansible-lint

  molecule:
    name: Molecule
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - name: Checkout the codebase
        uses: actions/checkout@v3

      - name: Setup Molecule
        uses: bec-galaxy/setup-molecule@v2

      - name: Run Molecule tests
        run: molecule test
```

Built-in Lint support has been [removed from Molecule](https://github.com/ansible-community/molecule/discussions/3825#discussioncomment-4908366).

> Environment variables `PY_COLORS` and `ANSIBLE_FORCE_COLOR` are optional and only activate the color output in the pipline.

## Packages

The following python packages are installed in this action:

- [ansible](https://pypi.org/project/ansible/)
- [molecule](https://pypi.org/project/molecule/)
- [molecule-plugins](https://pypi.org/project/molecule-plugins/)
- [docker](https://pypi.org/project/docker/)

> Package `molecule-plugins` is a breaking change and is required since `molecule` version 5.0.0.

## Molecule file

A Molecule sample file for this action.

```yaml
---
dependency:
  name: galaxy
driver:
  name: docker
platforms:
  - name: ubuntu-20.04
    image: ubuntu:20.04
  - name: ubuntu-22.04
    image: ubuntu:22.04
provisioner:
  name: ansible
  inventory:
    group_vars:
      all:
        ansible_user: root
scenario:
  name: default
```

## Licence

This project is licensed under MIT - See the [LICENSE](LICENSE) file for more information.

---

&uarr; [Back to top](#)