

![](../../common/images/customer.logo2.png)

# Microservices on Kubernetes and Autonomous Database

## Setting up a CI/CD environment for developing Microservices on an ATP Database, with Developer Cloud

## Introduction

This lab will walk you through the steps to set up a CI/CD environment for developing Microservices, based on the automation possible in Developer Cloud and deploying all components on Oracle's Managed Container platform and the ATP database.

## Prerequisites

To run these labs you will need access to an Oracle Cloud Account.  We have prepared this lab for the following situations: 

- <u>You are using your own Oracle Cloud Tenancy,</u> either via a **Trial**, using a **Pay-as-you-Go** account, or using the **Corporate account** of your organization.  If you do not have an account yet, you can obtain  a [Free Trial account](https://myservices.us.oraclecloud.com/mycloud/signup?sourceType=:eng:lw:ie::RC_EMMK190301P00254:220519_MicroATP).

  Next, follow the steps described on this page in the **Setup Steps** first and then **Configuration Labs** next

Please walk through the various labs in a sequential order, as the different steps depend on each other:

## Setup Steps

- **Step 1:** [Preparing your Tenancy for the lab](env-setup.md) .
- **Step 2:** [Provisioning an Autonomous Transaction Processing Database Instance](LabGuide100ProvisionAnATPDatabase.md)  using the OCI Console (manual).

You finished the setup part of your environment, and you can now start the configuratinn Labs

## Configuration Labs

- **Part 1:** [Setting up your Developer Cloud project](LabGuide250Devcs-proj.md)
- **Part 2:** [Build a Container image with the aone application running on ATP](LabGuide650BuildDocker.md)
- **Part 3:** [Spin up a Managed Kubernetes environment with Terraform](LabGuide660OKE_Create.md)
- **Part 4:** [Deploy your container on top of your Kubernetes Cluster](LabGuide670DeployDocker.md)

---
