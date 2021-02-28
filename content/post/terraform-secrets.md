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

In this article I'm going to cover several methods of storing and accessing secrets that can be applied easily and could also be used in other tools such as Ansible and even Bash.

Prerequisites

In order to try any technique laid out in this article you will need the following:

The [Terraform CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli), version 0.14 or later.

AWS Credentials [configured for use with Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication).

The [git CLI](https://git-scm.com/downloads).

To start login to the AWS Console and navigate to [AWS Secrets Manager](https://eu-west-1.console.aws.amazon.com/secretsmanager/home?region=eu-west-1#!/home) and click **Store a new secret**

Next select **Other type of secrets** followed by **Plaintext**.

![Store a new secret](./images/New_Secret.png)

Fill out the required secrets information and click **Next**

Now give your secret a name and maybe a description for future reference.

![Secret name and description](./images/secret_name_and_description.png)

Depending on your needs you can now set up auto rotation (an excellent idea for RDS) however for this example just leave it as disabled.

![Configure automatic rotation](./images/secret_rotation.png)

Check the **Review** page and if everything is ok click **Store**

So now that we have a stored secret we can start using it in our Terraform code with the ***aws_secretsmanager_secret_version*** parameter.


```terraform
data "aws_secretsmanager_secret_version" "idm_admin_password" {
  secret_id = "idm_admin_password"
}
```

Decode the json file (If you saved your secret as json that it :wink: ).

```terraform
locals {
  idm = jsondecode(data.aws_secretsmanager_secret_version.idm_admin_password.secret_string)
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
  username             = local.idm.username
  password             = local.idm.password
  parameter_group_name = "default.mysql5.7"
  skip_final_snapshot  = true
}
```

After doing a `terraform plan` you will see in the output that the password is marked as a **sensive value**

![Terraform plan output](./images/sensitive.png)

### Pros

* No plain text secrets are stored in terraform files or version control.
* Because everything is codified there's no need for external scripts or wrappers.
* AWS logs all transactions to the secrets manager so it's easy to see who did what and when.
* As I hinted at the start because the data is stored in AWS it can be accessed using the API so effectively any application can use the data not just Terraform.

### Cons
* Cost! At the time of writing is costs $0.40 per secret per month. For secrets that are stored for less than a month, the price is prorated (based on the number of hours.) And on top of that there's $0.05 per 10,000 API calls. [See this link for example costings](https://aws.amazon.com/secrets-manager/pricing/)


### Alternatives

You could use AWS SSM Parameter Store. It's very simple to use in Terraform and other tools including Ansible or even Bash scripts.

Start by navigating to [AWS Systems Manager](https://eu-west-1.console.aws.amazon.com/systems-manager/home?region=eu-west-1#) and look down the left hand menu for **Parameter Store**

Click **Create parameter** and give the parameter a name (for example username) and give it a value that suits you.

![Parameter store user](./images/parameter_store_username.png)

Do the same again for the password but this time select the **SecureString** option so that the contents are encrypted. For this you will need to choose an encryption method that fits your requirements. I'm choosing the default **KMS key source** in **My current account**.

![Encrypted password settings](./images/encrypted_ssm_password.png)


This is all that's required.

```terraform
data "aws_ssm_parameter" "user" {
  name            = "idm_admin_user"
  with_decryption = false
}

data "aws_ssm_parameter" "passwd" {
  name            = "idm_admin__password"
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
