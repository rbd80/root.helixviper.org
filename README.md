<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

# root.helixviper.org [![Latest Release](https://img.shields.io/github/release/rbd80/root.helixviper.org.svg)](https://github.com/rbd80/root.helixviper.org/releases)


Terraform Infrastructure for ("Rooting") Organization in AWS.

__NOTE:__ You need to provision the Root Organization first before create any other environments as this repo creates `dns` and `iam` resources needed for all sub accounts.


---


## Introduction

We use [geodesic](https://github.com/cloudposse/geodesic) to define and build world-class cloud infrastructures backed by AWS and powered by Kubernetes.

`geodesic` exposes many tools that can be used to define and provision AWS and Kubernetes resources.

Here is the list of tools we use to provision the `root.cloudposse.co` infrastructure:

* [aws-vault](https://github.com/99designs/aws-vault)
* [chamber](https://github.com/segmentio/chamber)
* [terraform](https://www.terraform.io/)


## Quick Start


### Setup AWS Role

__NOTE:__ You need to do it only once.

Configure AWS profile in `~/.aws/config`. 

```bash
[profile helixviper-root-admin]
region=us-east-1
role_arn=arn:aws:iam::323330167063:role/helixviper-root-admin
mfa_serial=arn:aws:iam::323330167063:mfa/helixviper-root-admin
source_profile=cpco
```

### Install and setup aws-vault

__NOTE:__ You need to do it only once.

We use [aws-vault](https://docs.cloudposse.com/tools/aws-vault/) to store IAM credentials in your operating system's secure keystore and then generates temporary credentials from those to expose to your shell and applications.

Install [aws-vault](https://docs.cloudposse.com/tools/aws-vault/) on your local computer first.

On MacOS, you may use `homebrew cask`

```bash
brew cask install aws-vault
```

Then setup your secret credentials for AWS in `aws-vault`
```bash
aws-vault add --backend file cpco
```

__NOTE:__ You should set `AWS_VAULT_BACKEND=file` in your shell rc config (e.g. `~/.bashrc`) so it persists.

For more info, see [aws-vault](https://docs.cloudposse.com/tools/aws-vault/)


## Examples

### Build Docker Image

```
# Initialize the project's build-harness
make init

# Build docker image
make docker/build
```

### Install the wrapper shell
```bash
make install
```

### Run the shell
```bash
root.helixviper.org
```

### Login to AWS with your MFA device
```bash
assume-role
```

__NOTE:__ Before provisioning AWS resources with Terraform, you need to create `tfstate-backend` first (S3 bucket to store Terraform state and DynamoDB table for state locking).

Follow the steps in this [README](https://github.com/cloudposse/terraform-root-modules/blob/master/aws/tfstate-backend/). You need to do it only once.

After `tfstate-backend` has been provisioned, follow the rest of the instructions in the order shown below.

### Provision `iam` with Terraform

Change directory to `iam` folder
```bash
cd /conf/iam
```

Because we don't have any roles created yet, Terraform will not be able to assume roles.

We need to provision `iam` with `assume_role` block commented out to create all roles and policies.

Comment out `assume_role` block in `iam/main.tf`.

Run Terraform
```bash
init-terraform
terraform plan
terraform apply
```

Uncomment `assume_role` block in `iam/main.tf`.

`exit` out of container session and run `assume-role` to pick up the new role.

```bash
assume-role
```

For more info, see [geodesic-with-terraform](https://docs.cloudposse.com/geodesic/module/with-terraform)


### Provision `dns` with Terraform

Change directory to `dns` folder
```bash
cd /conf/dns
```

Run Terraform
```bash
init-terraform
terraform plan
terraform apply
```

For more info, see [geodesic-with-terraform](https://docs.cloudposse.com/geodesic/module/with-terraform/)

### Provision `cloudtrail` with Terraform

```bash
cd /conf/cloudtrail
init-terraform
terraform plan
terraform apply
```




## Makefile Targets
```
Available targets:

  all                                 Initialize build-harness, install deps, build docker container, install wrapper script and run shell
  build                               Build docker image
  deps                                Install dependencies (if any)
  help                                This help screen
  help/all                            Display help for all targets
  install                             Install wrapper script from geodesic container
  push                                Push docker image to registry
  run                                 Start the geodesic shell by calling wrapper script

```



## Related Projects

Check out these related projects.

- [Packages](https://github.com/cloudposse/packages) - Cloud Posse installer and distribution of native apps
- [Build Harness](https://github.com/cloudposse/dev) - Collection of Makefiles to facilitate building Golang projects, Dockerfiles, Helm charts, and more
- [terraform-root-modules](https://github.com/cloudposse/terraform-root-modules) - Collection of Terraform "root module" invocations for provisioning reference architectures
- [audit.cloudposse.co](https://github.com/cloudposse/audit.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for an Audit Logs Organization in AWS.
- [prod.cloudposse.co](https://github.com/cloudposse/prod.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for a Production Organization in AWS.
- [staging.cloudposse.co](https://github.com/cloudposse/staging.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for a Staging Organization in AWS.
- [dev.cloudposse.co](https://github.com/cloudposse/dev.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for a Development Sandbox Organization in AWS.
- [testing.cloudposse.co](https://github.com/cloudposse/testing.cloudposse.co) - Example Terraform Reference Architecture that implements a Geodesic Module for an Automated Testing Organization in AWS
- [geodesic](https://github.com/cloudposse/geodesic) - 🚀 Geodesic is the fastest way to get up and running with a rock solid, production grade cloud platform built on strictly Open Source tools. 




## References

For additional context, refer to some of these links. 

- [Cloud Posse Documentation](https://docs.cloudposse.com) - Complete documentation for the Cloud Posse solution


## Help

**Got a question?**

File a GitHub [issue](https://github.com/rbd80/root.helixviper.org/issues), send us an [email][email] or join our [Slack Community][slack].

## Commerical Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide *commercial support* for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a fulltime engineer. 

[![E-Mail](https://img.shields.io/badge/email-hello@cloudposse.com-blue.svg)](mailto:hello@cloudposse.com)

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll develop original modules to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands on support to implement our reference architectures. 


## Community Forum

Get access to our [Open Source Community Forum][slack] on Slack. It's **FREE** to join for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build *sweet* infrastructure.

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/rbd80/root.helixviper.org/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://github.com/orgs/cloudposse/projects/3) with our other projects, we would love to hear from you! Shoot us an [email](mailto:hello@cloudposse.com).

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









### Credit to  
Inspired by the amazing work at [Cloud Posse, LLC][website].

