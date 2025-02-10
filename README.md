# Infrastruture As a Code

Earlier, it was a challenging and hectic job to manage IT infrastructures. The System Administrator had to manually manage all the underlying services, hardware, and software needed for the entire application system to work.

In recent years, the IT industry has seen a revolution in terms of tools and technologies that work towards automating the process of deploying and managing the Infrastructure. And this is what we term it as IaC.

Popular IaC tools include Terraform, AWS CloudFormation, Azure Resource Manager templates others.

These tools enable organizations to define, deploy, and manage their infrastructure efficiently and consistently, making it easier to adapt to the dynamic needs of modern applications and services.

## Issues with configuring infrastructure manually

1. Human Error: Manual configuration is prone to mistakes, such as incorrect settings, missed steps, or typos.

2. Inconsistency: It's challenging to ensure that configurations are identical across different environments (e.g., development, staging, production). Inconsistent setups can cause issues when deploying applications or scaling infrastructure.

3. Lack of Version Control: Manual configuration typically isn't tracked with version control systems like Git. This makes it hard to roll back to previous versions or understand who made which changes and why.

4. Limited Automation: Automation was limited to basic scripting, often lacking the robustness and flexibility offered by modern IaC tools.

5. Slow Provisioning: Provisioning new resources or environments was a time-consuming process that involved multiple manual steps, leading to delays in project delivery.

IaC addresses these challenges by providing a systematic, automated, and code-driven approach to infrastructure management.

# Terraform to the rescue

Terraform is an open-source infrastructure provisioning tool created by HashiCorp, it allows you to define and manage your infrastructure using a declarative language which is written in GO language. Terraform allows you to create, modify, and destroy infrastructure resources across different cloud providers.

There are multiple reasons why Terraform is used over the other IaC tools but below are the main reasons.

1. It supports multiple cloud providers such as AWS, Azure, Oracle, GCP, and more.

2. Terraform configuration is written in easy-to-understand language.

3. Terraform uses a declarative syntax, allowing you to specify the desired end-state of your infrastructure. This makes it easier to understand and maintain your code compared to imperative scripting languages.

4. Terraform maintains a state file that tracks the current state of your infrastructure. This state file helps Terraform understand the differences between the desired and actual states of your infrastructure, enabling it to make informed decisions when you apply changes.

5. Terraform can be integrated with other DevOps and automation tools, such as Docker, Kubernetes, Ansible, and Jenkins, allowing you to create comprehensive automation pipelines.

## HCL Basics

The HashiCorp Configuration Language (HCL) is a configuration language authored by HashiCorp.

### Key Elements of HCL

- HCL syntax comprises stanzas or blocks that define a variety of configurations available to Terraform. Stanzas or blocks are comprised of key = value pairs. Terraform accepts values of type string, number, boolean, map, and list.

- Single line comments start with #, while multi-line comments use an opening /_ and a closing _/.

- Lists of primitive types (string, number, and boolean) are wrapped in square brackets: ["Andy", "Leslie", "Nate", "Angel", "Chris"].

- Maps use curly braces {} and colons :, for example: { "password" : "my_password", "db_name" : "wordpress" }.

See Terraform’s [Configuration Syntax](https://developer.hashicorp.com/terraform/language/syntax/configuration) documentation for more details.

# Terraform Key Terminology

1. **Provider**: A provider is a plugin for Terraform that defines and manages resources for a specific cloud or infrastructure platform.
   Examples of providers include AWS, Azure, Google Cloud, and many others.
   You configure providers in your Terraform code to interact with the desired infrastructure platform.

2. **Resource**: A resource is a specific infrastructure component that you want to create and manage using Terraform. Resources can include virtual machines, databases, storage buckets, network components, and more. Each resource has a type and configuration parameters that you define in your Terraform code.

3. **Module**: A module is a reusable and encapsulated unit of Terraform code. Modules allow you to package infrastructure configurations, making it easier to maintain, share, and reuse them across different parts of your infrastructure. Modules can be your own creations or come from the Terraform Registry, which hosts community-contributed modules.

4. **Configuration File**: Terraform uses configuration files (often with a `.tf` extension) to define the desired infrastructure state. These files specify providers, resources, variables, and other settings. The primary configuration file is usually named `main.tf`, but you can use multiple configuration files as well.

5. **Variable**: Variables in Terraform are placeholders for values that can be passed into your configurations. They make your code more flexible and reusable by allowing you to define values outside of your code and pass them in when you apply the Terraform configuration.

6. **Output**: Outputs are values generated by Terraform after the infrastructure has been created or updated. Outputs are typically used to display information or provide values to other parts of your infrastructure stack.

7. **State File**: Terraform maintains a state file (often named `terraform.tfstate`) that keeps track of the current state of your infrastructure. This file is crucial for Terraform to understand what resources have been created and what changes need to be made during updates.

8. **Workspace**: Workspaces in Terraform are a way to manage multiple environments (e.g., development, staging, production) with separate configurations and state files. Workspaces help keep infrastructure configurations isolated and organized.

9. **Remote Backend**: A remote backend is a storage location for your Terraform state files that is not stored locally. Popular choices for remote backends include Amazon S3, Azure Blob Storage, or HashiCorp Terraform Cloud. Remote backends enhance collaboration and provide better security and reliability for your state files.

# Terraform Workflow

The workflow consists of lifecycle stages which start with the init, plan, apply, and destroy. All these have been executed as a command for the written code to update or changes in the infrastructure built.

![TFW](https://github.com/user-attachments/assets/54d124f0-667b-48af-ba95-2bd6d9c60608)


**Terraform Init**

- Terraform initializes the working directory containing terraform configuration files.

- Terraform initializes downloads and installs the provider’s plugin so that it can later be executed.

- Terraform has created a lock file .terraform.lock.hcl to record the provider selections it made above. Also, it initializes the backend configuration.

**Terraform Validate**

- The terraform validate command validates the configuration files in a directory.

- Validate command to verify whether a configuration is syntactically valid and thus primarily useful for general verification of reusable modules, including the correctness of attribute names and value types.

- It can run before the terraform plan. Validation requires an initialized working directory with any referenced plugins and modules installed.

**Terraform Plan**

- Terraform plan command is used to create an execution plan. It will not modify things in infrastructure. It compares the desired terraform state with the current state in the cloud. It doesn’t change the deployment.

- This command is a convenient way to check whether the execution plan for a set of changes matches your expectations without making any changes to the state

**Terraform Apply**

- It applies the changes required to reach the desired state of the configuration. This command executes the plan that is already created. It compares the correct state with desire state.

- It will write data into terraform.tfstate file. Once the application is completed, resources are immediately available.

**Terraform Destroy**

- The Terraform destroy command is used to delete the created resources in the Terraform-managed infrastructure.

# Terraform Configuration

Terraform script/code is stored in plain text file .tf extension, The file that contains terraform code is called a configuration file or terraform manifest.

Whenever you execute the terraform code you should be in the working directory where the configuration file is available.

## Terraform Configuration Syntax

```
     <BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>"   {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}

```

## Blocks and Arguments

Blocks are like a container to define other contents of your infrastructure.
An argument assigns a value to a particular name: argument name and argument value.
Argument names, block type names, and the names of most Terraform-specific constructs like resources, input variables, etc. are all identifiers.

In the terraform configuration file, most of the terraform features are controlled by top-level blocks. We need to have the following 3 blocks for creating a terraform configuration.

**Terraform Block** — Here is the required terraform version, list of providers required, and terraform backend.

```
terraform {
  required_providers {
    mycloud = {
      source  = "mycorp/mycloud"
      version = "~> 1.0"
    }
  }
}

```

**Provider Block** — It declares providers for Terraform to install the providers which is going to be used. Terraform relies on the providers to interact with remote systems. Provider configuration belongs to the root module.

```
# Configure the AWS Provider
provider "aws" {
  region = "us-east-1"
}

```

**Resource Block** — Each resource block describes one or more infrastructure objects. Each resource block is an infrastructure object, like compute instances, virtual networks, or other components.

```
resource "aws_instance" "webserver" {
ami = "ami_id"
instance_type = "t2.micro"
}
```
See Terraform’s [Providers and Resource](https://registry.terraform.io/namespaces/hashicorp) documentation for more details.

