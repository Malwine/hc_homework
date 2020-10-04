# Getting Started with Terraform

Terraform is a tool for defining and provisioning infrastructure as code (IaC) using the HashiCorp Configuration Language.
In this guide you will learn how to install Terraform, build and destroy basic infrastructure.

## Prerequisites
To follow this tutorial you will need:
- 

## Install Terraform

Visit [Terraform.io](https://www.terraform.io/downloads.html) to install Terraform. Choose the proper package for your platform, machine or environment. Download and unzip the package.

Finally, make sure that the terraform binary is available on your PATH. This process will differ depending on your operating system.

## Build infrastructure

With Terraform installed, let's start creating some infrastructure.

You may create a new directory on your local machine and create Terraform configuration code inside it.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following lines into the file.

```hcl
provider "docker" {
    host = "unix:///var/run/docker.sock"
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

Initialize Terraform with the `init` command. The AWS provider will be installed.

```shell
$ terraform init
```

You should check for any errors. If it ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The command will take up to a few minutes to run. Finally, it will display a message indicating that the resource was created.

## Destroy infrastructure

Finally, you may destroy the infrastructure.

```shell
$ terraform destroy
```

Confirm the message on the bottom by typing `yes` and hit ENTER. Terraform will destroy the resources it had created earlier.

## Next steps

- Find more guides and information on Terraform on the [Hashicorp learning Platform](https://learn.hashicorp.com/terraform).
- Learn how to use [Terraform with AWS](https://learn.hashicorp.com/collections/terraform/aws-get-started)
- Learn how to use [Terraform with  Azure](https://learn.hashicorp.com/collections/terraform/azure-get-started)
- Learn how to use [Terraform with  Google Cloud](https://learn.hashicorp.com/collections/terraform/gcp-get-started)
