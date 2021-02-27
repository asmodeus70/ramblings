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
