# My AWS Playground
My AWS playground to keep learning new AWS concepts.

## Table of Contents

- [My AWS Playground](#my-aws-playground)
  - [Table of Contents](#table-of-contents)
  - [**AWS: 7 Vital Concepts](#aws-7-vital-concepts)
    <details>
    <summary>Click to expand!</summary>

    - [EC2](#ec2)
    - [S3](#s3)
    - [SQS](#sqs) 
    - [Load Balancers](#load-balancers)
    - [API Gateways](#api-gateways)
    - [Lambda Functions](#lambda-functions)
    - [Amazon VPC](#amazon-vpc)
  
  - [**IaC: What is it and what are the options**](#iac-what-is-it-and-what-are-the-options)
    <details>
    <summary>Click to expand!</summary>

    - [IaC: What is it](#iac-what-is-it)
    - [IaC: What are the options](#iac-what-are-the-options)
      - [CloudFormation](#cloudformation)
        - [CloudFormation: Introduction](#cloudformation-introduction)  
      - [Terraform](#terraform)
        - [Terraform: Introduction](#terraform-introduction)  
      - [CDK](#cdk)
        - [CDK: Introduction](#cdk-introduction)  
  - [**IaC: Unit Testing**](#iac-unit-testing)
    <details>
    <summary>Click to expand!</summary>

    - [IaC: How to Unit Test?](#iac-how-to-unit-test)
      - [CloudFormation: Unit Testing](#cloudformation-unit-testing)
        - [CloudFormation: Demo](#cloudformation-demo)
        - [CloudFormation: Unit Tests](#cloudformation-unit-tests)
      - [Terraform: Unit Testing](#terraform-unit-testing)
        - [Terraform: Demo](#terraform-demo)
        - [Terraform: Unit Tests](#terraform-unit-tests)      
      - [CDK: Unit Testing](#cdk-unit-testing)
        - [CDK: Demo](#cdk-demo)
        - [CDK: Unit Tests](#cdk-unit-tests)      

## **AWS: 7 Vital Concepts**

Let’s face it, AWS can make you pull your hair out if you don’t understand what’s happening.

Scratch that, that’s programming in general.

What I’m about to share with you is basically what I wish I knew 4 years ago when I was working at a company as the only dev and they told me these exact words:

> “Hey V, We’ve decided to move to AWS, and the old dev quit, can you help”
>

Seems like a straightforward sentence, but what followed was a lot of stress. Stress because as someone who always did front end and some backend work, I wasn’t fully aware of deployment infrastructures or devops systems.
      
So this quick, and (what I think) simple guide, is to give you, an overview of AWS (conceptually) that I wish I had when I started — this is not a setup tutorial (that will come later).
      
### EC2

This is one of the building blocks of AWS. You will definitely interact with an EC2 instance at some point in your AWS journey provided you’re not going completely serverless (more on this later).

EC2 stands for Elastic Cloud Compute, and it’s an AWS service that provides you with a server (like a box, a MacBook without a screen) to run your application. You get to decide all sorts of configurations, memory, box size and power. But in short, it is a server with a public IP address (if you want it to be public) as well as an HTTP address

Once you build an EC2 instance, you can access it by SSHing into the box, i.e the equivalent of username and password into the server. Once inside you can do anything you want in a server

  * Run node jobs
  * Do a hello world application
  * Launch a server
  * Route your server localhost:3000 to the outside world using NGINX

PS if you’re wondering how the configuration is set up, AWS has this concept called Amazon Machine Images, which are basically “blueprints” for server configurations

You might wonder, who decides what data goes in/out of the server and this is dependant upon the security group your EC2 belongs to as well as the VPC ACL (this will be in a follow up blog)

PPS: With EC2 you can also run a “spot server”, let’s say you want to do a job once a week but don’t want to pay for the server the whole time, a spot server basically turns on, charges you for the time it’s operating, performs the task and then turns off. Saving you $$$.
      
### S3

### SQS

### Load Balancers

### API Gateways

### Lambda Functions

### Amazon VPC
      
**[⬆ back to top](#table-of-contents)**

## **IaC: What is it and what are the options**

### IaC: What is it

### IaC: What are the options](#iac-what-are-the-options)

#### CloudFormation

##### CloudFormation: Introduction

#### Terraform
  
##### Terraform: Introduction

#### CDK

##### CDK: Introduction

**[⬆ back to top](#table-of-contents)**
      
## **IaC: Unit Testing**      

### IaC: How to Unit Test?

#### CloudFormation: Unit Testing

##### CloudFormation: Demo

##### CloudFormation: Unit Tests

#### Terraform: Unit Testing

##### Terraform: Demo

##### Terraform: Unit Tests

#### CDK

##### CDK: Demo

##### CDK: Unit Tests

**[⬆ back to top](#table-of-contents)**
