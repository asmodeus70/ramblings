+++
title = "About"
description = "Who is Neil Radley?"
date = "2021-02-27"
aliases = ["about-me", "contact"]
author = "Neil Radley"
+++

## So you want to know about me

![:inline](images/nr.png)

**Contact** nradley@gmail.com

Hi and thanks for stopping by.

My name is Neil and I'm a senior Devops engineer at a UK based investment funding company. I've been in the IT industry for more years than I'm going to admit to here. :smile:

I specialise in Infrastructure as Code using Terraform and Ansible on AWS.

### **Professional Qualifications**

* AWS SysOps Associate

### **Enterprise Management Tools**

* Ansible
* Terraform
* Jenkins
* Amazon AWS EC2 & S3 storage, lambda, IAM
* Red Hat Satellite
* Docker
* Kubernetes
* Red Hat Identity Manager (IDM)
* Freeipa

### **Operating Systems Administration**

* Linux (Red Hat / CentOS Versions 6, 7 and 8 AWS Linux)
* Ubuntu 14.04, 16.04, 17.04, 18.04, 20.04

### **Programming/Scripting Languages**

* Shell Scripting (bash)
* Python
* Golang

### **Databases**

* Mysql / Percona
* Postgresql

### **Networking Technologies**

* TCP/IP
* LAN / WAN (PPP)

### **Methodologies**

* Agile

### **Experience**

#### **Crowdcube January 2020 - Present**

#### **Lead DevOps Engineer**

Heading up the Devops team, reporting to the CTO. I'm responsible for the AWS architecture design and implementation and overall cost savings to the company. These savings have been implemented by shutting down the dev and staging environments overnight and using AWS savings plans where ever possible. I've also been instrumental in reconfiguring the autoscaling processes so that it's possible to have a minimum number of instances running until such time as traffic increases over a pre-defined threshold where the autoscaling will scale up as required. I have also implemented a Jenkins CI/CD system allowing the developers to push new code automatically to stage and production by using Slack to raise a Jira ticket that triggers Jenkins to build a new AMI using Packer and Ansible which is then automatically deployed to AWS where the auto scaling groups are configured to do an instance refresh. I've also created several pipelines on Jenkins that allow the developers to do RDS database refreshes from production to stage from Slack while also running data cleansing to remove sensitive data. When I started at Crowdcube users were provisioned by adding public keys to Packer build images which meant that when someone started or left the company every single system needed a new AMI build to refresh the ssh keys. This was an untenable situation that required a new method of authentication. To that end I deployed FreeIPA in it's own management VPC allowing the central management of all users and systems. The entire platform was coded using Ansible to install and configure the FreeIPA software and Terraform to deploy the infrastructure which included everything from the load balancers, TLS keys, target groups and the route53 DNS records, new VPC, subnets, ACL's and instances.

#### **OnCorps April 2019 – December 2019**

#### **Lead DevOps Engineer**

Lead DevOps engineer reporting to and deputising for the CTO, I run a team of six offshore engineers and support a team of ten developers in Bristol and another four in Boston, Ohio. This role involves managing infrastructure in AWS using Terraform and Ansible to provision large scale artificial intelligence and deep learning systems. These systems have been configured to provide data scientists with tools such as RStudio and Jupyterhub with Aurora as the backend database and S3 for the training algorithms which is all provisioned using Terraform and Ansible. To provide end users with a web front end to access the data produced by the AI and deep learning systems I provisioned Elastic Beanstalk to deploy PHP dashboards.  I also manage large Kubernetes clusters and Docker environments and I was instrumental in configuring and deploying Rancher in high availability mode as the orchestration system for the Kubernetes clusters. All new code is automated through Jenkins and Spinnaker for deployment in production environments. I regularly use Packer to create AWS machine images (AMI) and Docker images which are pushed to the AWS Docker Registry for use in the clusters as well as for use by developers on their local systems. To help automate and perform routine tasks I write Python scripts or Go programmes along with bash scripts. I’ve also been pivotal in deploying AWS App Stream to enable streaming applications to end customers and setting up AWS Systems Manger (SSM) to allow patch management and general management of deployed systems. I’ve also deployed AWS Single Sign On (SSO) to all developers and engineers' access to AWS console applications such as CodeCommit and granular access to specific accounts for engineers.

#### **Aviva Insurance October 2018 – March 2019**

#### **Lead DevOps Engineer**

Lead DevOps engineer in charge of a team of 23 based both locally and in India. My duties include requirements gathering from key stakeholders and managers as well as developers in order to create meaningful Jira tickets that include all the relevant information required by engineers to fulfil the task as well as providing a definition of done by which the work can be assessed. I also perform quality assurance (QA) on all code and services produced by the team to ensure high standards are always met. Daily tasks involve writing Terraform and Ansible code to provision infrastructure in AWS as well as creating and maintaining Jenkins pipelines using Groovy. I’m also responsible for prioritising and scheduling work to engineers using Agile methodologies and sprint planning. Aviva also use Elastic Map Reduce (EMR) and Redshift for managing big data and data lakes for which I’m responsible for writing Terraform code to provision the environments which includes configuring data stores in S3 that can be used by Aviva partners all over the world to push data into the central data lake.

#### **Hodge Bank April 2018 – September 2018**

#### **Lead DevOps Engineer**

This role involves both management and DevOps as well as mentoring graduates. My duties involve creating
processes in Confluence for all aspects of AWS and Azure management from creating VPC's to groups and users. Part of This includes architecting the AWS and Azure infrastructure using best practices for infrastructure such as VPC creation, IAM roles and active directory integration and multi-factor authentication. I also write Terraform code to stand up AWS and Azure infrastructure for the bank’s bespoke java-based applications as well as for integrating active directory authentication using Azure AD. As the bank is evolving, I am also doing proof of concept monitoring demonstrations using Elasticsearch for the developers to be able to view errors and a Grafana logging system for Service Operations to view real time errors. I'm also responsible for developing the DevOps team by running a scrum team and generally nurturing best practices where ever possible. Most recently I have been building docker clusters using Kubernetes for orchestration to enable fast application scaling and allow for deployment using tools like Jenkins pipelines and git code repositories.

#### **Kainos (DVSA Project) October 2017 – March 2018**

#### **Role: Senior Devops Engineer**

This role involves writing Terraform and Puppet code to create microservices on AWS. Terraform is used to build the infrastructure (Launch Config, Auto Scaling Groups, Load Balancers both ALB and ELB, Target Groups, Lambda’s, VPC’s, subnets, S3 storage and IAM roles) and Puppet to install and configure the required software. All Terraform and Puppet code is pushed to a Git repository, and from there a Jenkins job runs to validate the code meets the required standard and also checks the functionality using Terraform plan or RSpec for Puppet. Some of the services use Packer to create a base image, followed by Ansible to bootstrap the system and deploy Puppet, which then configures the service. For both Packer and Ansible I’m responsible for writing and maintain the code as well as documenting the system on Confluence and tracking work on Jira. Another aspect of the role is deploying applications such as Consul and in-house bespoke applications to Docker using Kubernetes for container orchestration. Part of this role also involves coding using either Python or Ruby, as well as some bash scripting and also Groovy for managing the Jenkins multi branch pipelines.
