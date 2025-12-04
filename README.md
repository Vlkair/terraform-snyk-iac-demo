# Terraform + Snyk IaC Demo

This repository contains a minimal Terraform configuration that can be scanned using Snyk Infrastructure as Code (Snyk IaC) to demonstrate how a SAST tool detects security misconfigurations in IaC.

The configuration defines an AWS security group with an overly permissive ingress rule (`0.0.0.0/0`), which is a common issue that Snyk IaC flags as a high-severity finding.

## Prerequisites

- Terraform installed (version 1.0.0 or later)
- Snyk CLI installed and authenticated with a personal token
- An AWS account and credentials configured locally (for `terraform plan` if you want to generate a real plan)

## Files

- `main.tf`: Terraform configuration with an intentionally insecure security group

## Running a basic Snyk IaC scan

To scan the Terraform configuration files directly:

    snyk iac test


This command discovers Terraform `.tf` files in the current directory and analyzes them for misconfigurations.

You should see at least one issue related to an overly permissive security group ingress rule.

## Scanning a Terraform plan with Snyk IaC (optional)

You can also scan a Terraform plan in JSON format to see the planned changes:

1. Initialize the Terraform working directory:  
   
        terraform init a


2. Create a binary plan file:

        terraform plan -out=tfplan.binary

3. Convert the plan to JSON:

       terraform show -json tfplan.binary > tf-plan.json

4. Scan the JSON plan with Snyk IaC:

       snyk iac test tf-plan.json


By analyzing the plan, Snyk IaC inspects the final planned state, including resources, modules, and variable values, before any changes are applied.

## Purpose

This repository is intended as a simple example to accompany an article about using Snyk IaC as a SAST tool for Terraform. It demonstrates the basic workflow:

- Write Terraform code
- Run `snyk iac test` to detect misconfigurations
- Fix the code and re-run the scan to verify that the issues are resolved

