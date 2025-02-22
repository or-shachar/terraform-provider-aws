# Development Environment Setup

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) 0.12.26+ (to run acceptance tests)
- [Go](https://golang.org/doc/install) 1.20+ (to build the provider plugin)

## Quick Start

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (please check the [requirements](#requirements) before proceeding).

*Note:* This project uses [Go Modules](https://blog.golang.org/using-go-modules) making it safe to work with it outside of your existing [GOPATH](http://golang.org/doc/code.html#GOPATH). The instructions that follow assume a directory in your home directory outside of the standard GOPATH (i.e `$HOME/development/hashicorp/`).

Clone repository to: `$HOME/development/hashicorp/`

```console
$ mkdir -p $HOME/development/hashicorp/; cd $HOME/development/hashicorp/
$ git clone git@github.com:hashicorp/terraform-provider-aws
...
```

Enter the provider directory and run `make tools`. This will install the needed tools for the provider.

```console
$ make tools
```

To compile the provider, run `make build`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

```console
$ make build
...
$ $GOPATH/bin/terraform-provider-aws
...
```

## Testing the Provider

In order to test the provider, you can run `make test`.

*Note:* Make sure no `AWS_ACCESS_KEY_ID` or `AWS_SECRET_ACCESS_KEY` variables are set, and there's no `[default]` section in the AWS credentials file `~/.aws/credentials`.

```console
$ make test
```

In order to run the full suite of Acceptance tests, run `make testacc`.

*Note:* Acceptance tests create real resources, and often cost money to run. Please read [Running and Writing Acceptance Tests](running-and-writing-acceptance-tests.md) in the contribution guidelines for more information on usage.

```console
$ make testacc
```

## Using the Provider

With Terraform v0.14 and later, [development overrides for provider developers](https://www.terraform.io/cli/config/config-file#development-overrides-for-provider-developers) can be leveraged in order to use the provider built from source.

To do this, populate a Terraform CLI configuration file (`~/.terraformrc` for all platforms other than Windows; `terraform.rc` in the `%APPDATA%` directory when using Windows) with at least the following options:

```terraform
provider_installation {
  dev_overrides {
    "hashicorp/aws" = "[REPLACE WITH GOPATH]/bin"
  }
  direct {}
}
```
