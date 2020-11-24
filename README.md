<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS IAM Role
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module to create Iam role resource on AWS.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v0.13-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/devops4mecode/terraform-aws-iam-role'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+IAM+Role&url=https://github.com/devops4mecode/terraform-aws-iam-role'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+IAM+Role&url=https://github.com/devops4mecode/terraform-aws-iam-role'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>

## Prerequisites

This module has a few dependencies: 

- [Terraform 0.13](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)

## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/devops4mecode/terraform-aws-iam-role/releases).


### Simple example
Here is an example of how you can use this module in your inventory structure:
```hcl
      module "iam-role" {
      source      = "devops4mecode/iam-role/aws"
      version     = "1.0.0"

      name               = "iam-role"
      application        = "devops4me"
      environment        = "test"
      label_order        = ["environment", "application", "name"]
      assume_role_policy = data.aws_iam_policy_document.default.json

      policy_enabled = true
      policy         = data.aws_iam_policy_document.iam-policy.json
    }

      data "aws_iam_policy_document" "default" {
      statement {
      effect  = "Allow"
      actions = ["sts:AssumeRole"]
      principals {
      type        = "Service"
      identifiers = ["ec2.amazonaws.com"]
      }
      }
    }

      data "aws_iam_policy_document" "iam-policy" {
      statement {
      actions = [
      "ssm:UpdateInstanceInformation",
      "ssmmessages:CreateControlChannel",
      "ssmmessages:CreateDataChannel",
      "ssmmessages:OpenControlChannel",
      "ssmmessages:OpenDataChannel"    ]
      effect    = "Allow"
      resources = ["*"]
      }
    }
```
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| application | Application (e.g. `do4m` or `devops4me`). | `string` | `""` | no |
| assume\_role\_policy | Whether to create Iam role. | `any` | n/a | yes |
| attributes | Additional attributes (e.g. `1`). | `list` | `[]` | no |
| delimiter | Delimiter to be used between `organization`, `environment`, `name` and `attributes`. | `string` | `"-"` | no |
| description | The description of the role. | `string` | `""` | no |
| enabled | Whether to create Iam role. | `bool` | `true` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| force\_detach\_policies | The policy that grants an entity permission to assume the role. | `bool` | `false` | no |
| label\_order | Label order, e.g. `name`,`application`. | `list` | `[]` | no |
| managedby | ManagedBy, eg 'DevOps4Me' or 'NajibRadzuan'. | `string` | `"najibradzuan@devops4me.com"` | no |
| max\_session\_duration | The maximum session duration (in seconds) that you want to set for the specified role. If you do not specify a value for this setting, the default maximum of one hour is applied. This setting can have a value from 1 hour to 12 hours. | `number` | `3600` | no |
| name | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| path | The path to the role. | `string` | `"/"` | no |
| permissions\_boundary | The ARN of the policy that is used to set the permissions boundary for the role. | `string` | `""` | no |
| policy | The policy document. | `any` | `null` | no |
| policy\_arn | The ARN of the policy you want to apply. | `string` | `""` | no |
| policy\_enabled | Whether to Attach Iam policy with role. | `bool` | `false` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | `map` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| arn | The Amazon Resource Name (ARN) specifying the role. |
| name | Name of specifying the role. |
| tags | A mapping of tags to assign to the resource. |

## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system. 

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```