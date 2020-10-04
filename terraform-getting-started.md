# Getting Started with Terraform

Terraform is a tool for defining and provisioning infrastructure as code (IaC) using the HashiCorp Configuration Language.
In this guide you will learn how to install Terraform, build and destroy basic infrastructure.

## Prerequisites
To follow this tutorial you will need:
- [Docker](https://www.docker.com/)

## Install Terraform

Visit [Terraform.io](https://www.terraform.io/downloads.html) to install Terraform. Choose the proper package for your platform, machine or environment. Download and unzip the package.

Finally, make sure that the terraform binary is [available on your PATH](https://learn.hashicorp.com/tutorials/terraform/install-cli?in=terraform/azure-get-started). This process will differ depending on your operating system.

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

Initialize Terraform with the `init` command. The Docker provider will be installed.

```shell
$ terraform init
```

Your output should look similar to the one below.

```shell
Initializing the backend...

Initializing provider plugins...
- Finding latest version of terraform-providers/docker...
- Installing terraform-providers/docker v2.7.2...
- Installed terraform-providers/docker v2.7.2 (signed by HashiCorp)

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, we recommend adding version constraints in a required_providers block
in your configuration, with the constraint strings suggested below.

* terraform-providers/docker: version = "~> 2.7.2"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

You should check for any errors. If it ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```
Confirm to perform the actions by typing `yes` and hit ENTER. Your output should look similar to the one below.

```shell
docker_image.nginx: Refreshing state... [id=sha256:7e4d58f0e5f3b60077e9a5d96b4be1b974b5a484f54f9393000a99f3b6816e3dnginx:latest]
docker_container.nginx: Refreshing state... [id=c94b0979a081a39f25e9b38c0d588458834eaea66c4134374c9bcf397b1bfb26]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach           = false
      + bridge           = (known after apply)
      + command          = (known after apply)
      + container_logs   = (known after apply)
      + dns              = (known after apply)
      + dns_opts         = (known after apply)
      + entrypoint       = (known after apply)
      + exit_code        = (known after apply)
      + gateway          = (known after apply)
      + hostname         = (known after apply)
      + id               = (known after apply)
      + image            = "sha256:7e4d58f0e5f3b60077e9a5d96b4be1b974b5a484f54f9393000a99f3b6816e3d"
      + ip_address       = (known after apply)
      + ip_prefix_length = (known after apply)
      + ipc_mode         = (known after apply)
      + log_driver       = (known after apply)
      + log_opts         = (known after apply)
      + logs             = false
      + must_run         = true
      + name             = "training"
      + network_data     = (known after apply)
      + read_only        = false
      + restart          = "no"
      + rm               = false
      + shm_size         = (known after apply)
      + start            = true
      + user             = (known after apply)
      + working_dir      = (known after apply)

      + ports {
          + external = 4000
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 1s [id=f354c37ff6b193b3c2fef9a65314cd165cab7294a1670633960bfe98a733d401]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

The command will take up to a few minutes to run. Finally, it will display a message indicating that the resource was created.

## Destroy infrastructure

To destroy the infrastructure use the `destroy` command.

```shell
$ terraform destroy
```

Confirm the destruction of all resources by typing `yes` and hit ENTER. Terraform will destroy the resources it had created earlier.

```shell
docker_image.nginx: Refreshing state... [id=sha256:7e4d58f0e5f3b60077e9a5d96b4be1b974b5a484f54f9393000a99f3b6816e3dnginx:latest]
docker_container.nginx: Refreshing state... [id=f354c37ff6b193b3c2fef9a65314cd165cab7294a1670633960bfe98a733d401]

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach            = false -> null
      - command           = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - cpu_shares        = 0 -> null
      - dns               = [] -> null
      - dns_opts          = [] -> null
      - dns_search        = [] -> null
      - entrypoint        = [
          - "/docker-entrypoint.sh",
        ] -> null
      - gateway           = "172.17.0.1" -> null
      - group_add         = [] -> null
      - hostname          = "f354c37ff6b1" -> null
      - id                = "f354c37ff6b193b3c2fef9a65314cd165cab7294a1670633960bfe98a733d401" -> null
      - image             = "sha256:7e4d58f0e5f3b60077e9a5d96b4be1b974b5a484f54f9393000a99f3b6816e3d" -> null
      - ip_address        = "172.17.0.2" -> null
      - ip_prefix_length  = 16 -> null
      - ipc_mode          = "private" -> null
      - links             = [] -> null
      - log_driver        = "json-file" -> null
      - log_opts          = {} -> null
      - logs              = false -> null
      - max_retry_count   = 0 -> null
      - memory            = 0 -> null
      - memory_swap       = 0 -> null
      - must_run          = true -> null
      - name              = "training" -> null
      - network_data      = [
          - {
              - gateway          = "172.17.0.1"
              - ip_address       = "172.17.0.2"
              - ip_prefix_length = 16
              - network_name     = "bridge"
            },
        ] -> null
      - network_mode      = "default" -> null
      - privileged        = false -> null
      - publish_all_ports = false -> null
      - read_only         = false -> null
      - restart           = "no" -> null
      - rm                = false -> null
      - shm_size          = 64 -> null
      - start             = true -> null
      - sysctls           = {} -> null
      - tmpfs             = {} -> null

      - ports {
          - external = 4000 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id     = "sha256:7e4d58f0e5f3b60077e9a5d96b4be1b974b5a484f54f9393000a99f3b6816e3dnginx:latest" -> null
      - latest = "sha256:7e4d58f0e5f3b60077e9a5d96b4be1b974b5a484f54f9393000a99f3b6816e3d" -> null
      - name   = "nginx:latest" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.nginx: Destroying... [id=f354c37ff6b193b3c2fef9a65314cd165cab7294a1670633960bfe98a733d401]
docker_container.nginx: Destruction complete after 1s
docker_image.nginx: Destroying... [id=sha256:7e4d58f0e5f3b60077e9a5d96b4be1b974b5a484f54f9393000a99f3b6816e3dnginx:latest]
docker_image.nginx: Destruction complete after 0s

Destroy complete! Resources: 2 destroyed.
```

## Next steps

- Find more guides and information on Terraform on the [Hashicorp learning Platform](https://learn.hashicorp.com/terraform).
- Learn how to use [Terraform with AWS](https://learn.hashicorp.com/collections/terraform/aws-get-started)
- Learn how to use [Terraform with  Azure](https://learn.hashicorp.com/collections/terraform/azure-get-started)
- Learn how to use [Terraform with  Google Cloud](https://learn.hashicorp.com/collections/terraform/gcp-get-started)
