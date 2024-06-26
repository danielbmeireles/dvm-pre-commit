# DevOps Meetup: pre-commit hooks + IaC Static Code Analysis Tools

This document contains instructions on configuring the **pre-commit** framework for this repository, with contain numerous terraform code examples.

Also, we will explore two open-sorce, community-driven static code analysis tool for scanning infrastructure as code (IaC) files for misconfigurations called **Checkov** and **Terrascan**.

This repository is a fork from [alfonsof/terraform-azure-examples](https://github.com/alfonsof/terraform-azure-examples]).

## First things first!

### Clone the repo

```bash
$ gh repo clone danielbmeireles/dvm-pre-commit
```

### Set the default remote repo

```bash
$ gh repo set-default

This command sets the default remote repository to use when querying the
GitHub API for the locally cloned repository.

gh uses the default repository for things like:

 - viewing and creating pull requests
 - viewing and creating issues
 - viewing and creating releases
 - working with GitHub Actions
 - adding repository and environment secrets

? Which repository should be the default? danielbmeireles/dvm-pre-commit
✓ Set danielbmeireles/dvm-pre-commit as the default repository for the current directory
```

### Inspect the pre-commit config file

```bash
$ yq .pre-commit-config.yaml
```

Can you identify the three main sections of the file? How many repos are configured? And how many hooks?

### Install the hooks

```bash
$ pre-commit install --install-hooks
```

### Update the hooks

```bash
$ pre-commit autoupdate
```

Does any repository was updated?

## Playing with the pre-commit command

### Manually run pre-commit hooks

At any time, you can manually run all pre‑commit hooks in a repository. For example, following some code modifications but prior to committing your changes, you can run the hooks to reveal any identified issues beforehand. Just run the following command:

```bash
$ pre-commit run
```

Bear in mind that this checks only for files added with `git add`.

### Running an individual hook by referring its ID

```bash
$ pre-commit run terraform-fmt
```

### Check all files in the repo

If you want to check all files in the re­pository, regardless of their state in the Git database, add the `‑‑all‑files` argument:

```bash
$ pre-commit run --all-files
```

This is always a good idea after adding a new hook. You can also combine this with the restriction to an individual hook:

```bash
$ pre-commit run terraform-fmt --all-files
```

### Skipping a failed hook

```bash
$ SKIP=checkov git commit ‑m "Add foo"
```

### Skipping multiple hooks

```bash
$ SKIP=checkov,terrascan git commit ‑m "Add foo"
```

### Skipping all hooks

```bash
$ git commit ‑m "Add foo" ‑‑no‑verify
```

## Playing with the checkov command

### Select a single file and scan

```bash
$ checkov -f main.tf
```

### Select input folder and scan

```bash
$ checkov -d /user/tf
```

## Playing with the terrascan command

### Initializing Terrascan

```bash
$ terrascan init
```

Note: The init command is implicitly executed if the scan command does not find policies while executing.

### Terrascanning

```bash
$ terrascan scan
```

### Terrascanning current directory containing terraform files for AWS Resources

```bash
$ terrascan scan -t aws
```

Try to execute the same command but using the `azure` cloud provider.

### Terrascanning for a specific IaC provider

```bash
$ terrascan scan -i terraform
```

### Remote terrascanning

```bash
$ terrascan scan -t azure -r git -u git@github.com:danielbmeireles/dvm-pre-commit.git//code/01-hello-world
```
