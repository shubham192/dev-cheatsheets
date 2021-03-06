---
title: CLI usage
description: How to run Terraform commands in the terminal
layout: listing
---

## Basics

Follow these three commands in order as a started for using Terraform.

1. Install dependencies.
    ```sh
    $ terraform init
    ```
2. Preview changes.
    ```sh
    $ terraform plan
    ```
3. Deploy changes.
    ```sh
    $ terraform apply .
    ```


## Development

Format the Terraform code.

```sh
$ terraform fmt
```

Validate the Terraform code.

```sh
$ terraform validate
```


## Alias

Recommended for shell config:

```sh
alias tf=terraform
```
