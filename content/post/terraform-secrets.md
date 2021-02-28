+++
author = "Neil Radley"
title = "Managing secrets in Terraform"
date = "2021-02-27"
description = "Methods of safely using secrets in Terraform"
featured = true
tags = [
    "terraform",
    "secrets"
]
categories = [
    "Terraform"
]
series = ["Terraform"]
aliases = ["terraform secrets"]
thumbnail = "images/top_secret.webp"
+++

Managing secrets in Terraform can be daunting especially when plans are stored in plain text. But it doesn't have to be this way.
<!--more-->

In this article I'm going to cover severlal methods of storing and accessing secrets that can be applied easily and could also be used in other tools such as Ansible and even Bash.

Prerequisites

In order to follow this tutorial, you will need the following:

The [Terraform CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli), version 0.14 or later.

AWS Credentials [configured for use with Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication).

The [git CLI](https://git-scm.com/downloads).


```terraform
data "aws_secretsmanager_secret_version" "db_passwd" {
  secret_id = "idm_admin_password.id"
}
```

Decode the json file.

```terraform
output "secrets" {
  value = jsondecode(data.aws_secretsmanager_secret_version.db_passwd.secret_string)
  sensitive = true
}
```

**NOTE:** The **sensitive** option is only available in Terraform 0.14 and above

```terraform
resource "aws_db_instance" "primary" {
  allocated_storage    = 10
  engine               = "mysql"
  engine_version       = "5.7"
  instance_class       = "db.t3.medium"
  name                 = "Craft"
  username             = module.secrets.username
  password             = module.secrets.password
  parameter_group_name = "default.mysql5.7"
  skip_final_snapshot  = true
}
```

### Pros

* No plain text secrets are stored in terraform files or version control.
* Because everything is codified there's no need for external scripts or wrappers.
* AWS logs all transactions to the secrets manager so it's easy to see who did what and when.
* As I hinted at the start because the data is stored in AWS it can be accessed using the API so effectively any application can use the data not just Terraform.

### Cons
* Cost! At the time of writing is costs $0.40 per secret per month. For secrets that are stored for less than a month, the price is prorated (based on the number of hours.) And on top of that there's $0.05 per 10,000 API calls. [See this link for example costings](https://aws.amazon.com/secrets-manager/pricing/)


### Alternatives

You could use AWS SSM Parameter Store. It's very simple to use in Terraform and other tools including Ansible or even Bash scripts.

This is all that's required.

```terraform
data "aws_ssm_parameter" "user" {
  name            = "db_user"
  with_decryption = true
}

data "aws_ssm_parameter" "passwd" {
  name            = "db_passwd"
  with_decryption = true
}
```

And then use it like this.

```terraform
resource "aws_db_instance" "primary" {
  allocated_storage    = 10
  engine               = "mysql"
  engine_version       = "5.7"
  instance_class       = "db.t3.medium"
  name                 = "Craft"
  username             = data.aws_ssm_parameter.user
  password             = data.aws_ssm_parameter.passwd
  parameter_group_name = "default.mysql5.7"
  skip_final_snapshot  = true
}
```

The first thing to notice is that there are now two entries. One for the user and one for the password. Ok so it's a bit more coding but it is elegant.
