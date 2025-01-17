# Contributing

Want to work on Akri with us? 🎉 We are actively looking for new contributors along all stages of the contribution journey, from casual contributors to reviewers and maintainers.

## What do I need to know to help?

Akri utilizes a variety of technologies, and different background knowledge is more or less useful depending on what you are interested in contributing.

* Some understanding of [Kubernetes](https://kubernetes.io/) and [Helm](https://helm.sh/) is needed to deploy and use Akri. 
* The Akri Controller and Agent are written in the [Rust programming language](https://www.rust-lang.org/learn). 
* All of Akri's components run on Linux, so you will need to set up an Ubuntu VM if you do not already have a Linux environment. 
* [Sample brokers](../development/broker-development.md) and end applications can be written in any language and are individually containerized.
* [Discovery handlers](../development/handler-development.md) can be written in any language and can be deployed in their own Pods. However, if you would like your discovery handler to be embedded in the Akri Agent Pods, it must be written in Rust. 
* We use Docker to build our [containers](https://www.docker.com/resources/what-container).

## How do I get started developing?

Contributions can be made by forking the repository and creating a pull request. Ideally, every pull request should have a corresponding issue that it is resolving. Each pull request will kick off a set of CI builds to validate that:

* the code adheres to standard Rust formatting (`cargo fmt`)
* the code builds properly (`cargo build`)
* the code is free of common mistakes (`cargo clippy`)
* the Akri tests all pass (`cargo test`)
* the inline documentation builds (`cargo doc`)

See the [**developer guide**](../development/development.md) for more information on how to set up your environment and build Akri components locally.

## Versioning

We follow the [SymVer](https://semver.org/) versioning strategy: \[MAJOR\].\[MINOR\].\[PATCH\]. Our current version can be found in `version.txt`.

* For non-breaking bug fixes and small code changes, \[PATCH\] should be incremented.  This can be accomplished by running `./version.sh -u -p`
* For non-breaking feature changes, \[MINOR\] should be incremented.  This can be accomplished by running `./version.sh -u -n`
* For major and/or breaking changes, \[MAJOR\] should be incremented.  This can be accomplished by running `./version.sh -u -m`

To ensure that all product versioning is consistent, our CI builds will execute `./version.sh -c` to check all known instances of version in our YAML, TOML, and code. This will also check to make sure that version.txt has been changed. If a pull request is needed where the version should not be changed, add `same version` label to the pull request by commenting `/add-same-version-label`.

> Note for MacOS users: `version.sh` uses the GNU `sed` command under-the-hood, but MacOS has built-in its own version. We recommend installing the GNU version via `brew install gnu-sed`. Then follow the brew instructions on how to use the installed GNU `sed` instead of the MacOS one.

Alternatively, you could skip running `version.sh` file altogether. Once you make a pull request, you should comment any one of:

* `/add-same-version-label` to not change version
* `/version patch` for non-breaking bug fixes and small code changes
* `/version minor` for non-breaking feature changes
* `/version major` for major and/or breaking changes

Commenting these commmands on your pull request will automatically update the version for you and push the changes to your pull request branch.

## Logging

Akri follows similar logging conventions as defined by the [Tracing crate](https://docs.rs/tracing/0.1.22/tracing/struct.Level.html). When adding logging to new code, follow the verbosity guidelines.

| verbosity | when to use? |
| :--- | :--- |
| error | Unrecoverable fatal errors |
| warn | Unexpected errors that may/may not lead to serious problems |
| info | Useful information that provides an overview of the current state of things (ex: config values, state change) |
| debug | Verbose information for high-level debugging and diagnoses of issues |
| trace | Extremely verbose information for developers of Akri |

## PR labels
Akri's workflows check for two labels in the PRs in order to decide whether to execute certain checks. 

The [version check workflow](https://github.com/project-akri/akri/blob/main/.github/workflows/check-versioning.yml) will run, ensuring you have increased the version number, unless you (A) only change a file that is on an ignored path of the workflow, such as all `*.md` files OR (B) add the `same version` label to the pull request. Use this label if your change will trigger the workflow and the version should not be changed by your PR. The label will cause the check to automatically succeed.

Akri has some intermediate containers that decrease the build time of the more frequently built final containers. These intermediate builds are long running and should only be run when absolutely needed. If your PR triggers a workflow to build them, you will see the workflow fail and get a message that requests that you add `build dependency containers` label to your PR to start the build. 

You can add labels by commenting:
  - `/add-build-dependency-containers-label`
  - `/add-same-version-label`

## CLA

Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit [https://cla.microsoft.com](https://cla.microsoft.com).

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

## Code of Conduct

Participation in the Akri community is governed by the [Code of Conduct](../../CODE_OF_CONDUCT.md).

