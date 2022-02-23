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
      - [Terraform vs CloudFormation vs AWS CDK](#terraform-vs-cloudformation-vs-aws-cdk)
      - [Other IaC tools and Terraform alternatives](#other-iac-tools-and-terraform-alternatives)
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

S3 is fantastic if you treat it right. Amazon S3 stands for Amazon Simple Storage Service (hope you’re picking up their vibe with numbers in the abbreviations)

S3 is a programmatic dropbox. You can upload photos, videos, JSON, gzips, entire frontend web projects and let it get served via a public URL. It is also used for holding versions of your server when you’re trying to auto-deploy your server using github or bitbucket (more on this later) — basically, it can host a heap of different s**t

The most common uses I’ve had for S3 have been 2 fold. One to host assets uploaded by users (if your customers upload a profile photo etc for example) and the second to serve my actual frontend website.

See S3 has this magical feature where it lets you upload the (for eg) the dist file of your Vue/React/Angular project into an S3 bucket and serve it to your customers. You can do this quite literally by routing your S3 URL (which they create for you automatically) with a CNAME you set up on godaddy or any hosting service.

In order for you to “Authenticate” or “secure (put https)” your S3 bucket website URL, you’ll need to associate it with something called CloudFront (I know, F me so many things) which is Amazons CDN network, this service allows for you connect your actual custom domain “banana.com” to the S3 bucket by providing the S3 bucket as an “origin”.

I won’t go into the benefits of a CDN, so if your S3 bucket is a public-facing bucket, I wouldn’t see why you wouldn’t make it part of a CDN network (content delivery network) to pace up asset delivery

### SQS

Amazon has its own service (of course) for message queues. If you’re not completely aware of what a message queue is, here’s my way of understanding it.

If you’ve ever stood in line at a McDonalds, you see this little holding area where there are bags of food sitting around waiting to be distributed by a staff member.

That is the queue, and the message (i.e the food) can only be processed once (i.e once a message to make food, or once the food is given to the customer, that’s it)

Message queues are a form of async communication, the main role of Message Queues is to batch large loads of work, smoothen spiky workloads, and decouple heavyweight tasks (large cron job processing)

![Image Credit AWS](./assets/SQS.jpeg)

Queue services are used extensively in modern architecture to speed up application build and also simplify the process of building apps. Modern-day builds include several micro-services that are isolated from each other and SQS allows for data to be transferred from a producer (the one sending a message) to the consumer (the receiver) in fast and effective way. Since its async, there are no “thread blockages” that happen therefore stopping the entire service.

Going back to the McDonalds example, imagine how crap the service would be if only one order can be delivered at a time, and until one order is delivered the other can begin.

The process effectively works by sending and receiving message signals, the producer sends a message by adding a task to the queue (putting an order on the delivery table at an McDs) the message sits on that table until a receiver takes the message and does something with it (give it to the customer)

You might ask, okay how does this work when there are one producer and many receivers, this is called a Pub/Sub system (Publish/Subscribe)

An example would be, if a sale is made on a Shopify store, there would be multiple services hooked into that “topic of a sale” to perform multiple, different/isolated tasks. For eg. Send a Slack notification to the shop owner, print out an order label, trigger an email sequence.

### Load Balancers

The name says it all, a Load Balancer’s job is to sit on top of a network of (for this example) EC2 boxes and check to see if each server is currently on overload or not.

If a server is on overload, the job of the load balancer is to divert traffic to the next closest available server.

You might wonder, wait what if I have an open socket with a server behind the load balancer, how is that session magically maintained/transferred across to a whole new server running in parallel. The answer is if you do have situations like this, AWS Application Load Balancer is smart enough to sustain ongoing sessions (Just need to tick the make it sticky checkbox when creating a load balancer)

Another use case of load balancers is that they provide you with a SSL certified endpoint (don’t need to add your own at least during testing), you can expose this route via a CNAME or a Masked route ( https://server.myapp.com). At this point, you need to make sure your EC2 instances are only accessible internally (i.e remove any external IP access), this will make sure that any security threat is isolated to minimal points of entry

### API Gateways

I learnt of API gateways during my quest to set up an SSL for an EC2 server. The first attempt was painful, I tried doing it within the EC2 instance, I was breaking my head (in hindsight, I overcomplicated things) but as a happy surprise, I came to learn of API gateways.

Think of an API gateway as a proxy, i.e its the middleman that receives your requests, do something to it if you want, and then sends that request to someone else you have no clue about.

There are many use-cases for API Gateways, but the 2 I’m mentioning, in particular, are acting as a secure proxy for an EC2 instance and second, wrapping a request with auth tokens.

Have you ever had that experience where you might need to make a request from the front end to a 3rd-party service, but the only way you can access that service is by adding to the request header an auth token, but that auth token is sensitive. You might think you need to go ahead and build an entire server to receive these requests, amend it and then send it to the 3rd party API. That’s a very painful way, an easier way is using an API gateway, where it gives you the capability to mutate the request (in a limited way) before you send it off to the 3rd party API.

### Lambda Functions

AWS Lambda functions let you run “functions” in the cloud without needing to maintain a server. The function executes your code only when you need it to (certain time of day, or when it receives a request from somewhere) and it can scale really fast!

The common use I’ve seen is mainly to respond to changes in your DB, react to HTTP requests it receives from AWS API gateway.

So you can treat lambda functions as part of a “serverless” architecture.

Supply the code to a lambda function, tell it what event it needs to react to and let it run free.

### Amazon VPC

A Virtual Private Cloud is a private cloud within AWS’ public cloud. Think of it as your own little office space inside a WeWork (LOL) which is publically accessible to everyone

Within that room, you’ve got own systems set up your own processes and communication layer, however, it can only be accessed via a restricted endpoint i.e the front-door.
      
**[⬆ back to top](#table-of-contents)**

## **IaC: What is it and what are the options**

### IaC: What is it

Infrastructure as code (IaC) helps organizations achieve their DevOps goals of automation and self-service by maintaining declaration files in version control that define your application environments. This practice supports programmatic build, configuration, and destruction of environments while enabling self-service, reducing errors, and eliminating time-consuming rollbacks.

### IaC: What are the options](#iac-what-are-the-options)

IT teams have two AWS-native options for infrastructure as code -- AWS CloudFormation and the AWS Cloud Development Kit (CDK).

  * **AWS CloudFormation:** CloudFormation templates were AWS' first foray into cloud-based infrastructure as code, and while still useful, CloudFormation has clear weaknesses. More specifically, it doesn't offer built-in logic capabilities and has a steep learning curve.

  * **AWS CDK:** The AWS CDK, an open source software development framework to define cloud infrastructure, addresses these weaknesses. The AWS CDK supports popular programming languages, which developers can use to build, automate and manage infrastructure based on an imperative approach.

  * **Terraform:** Terraform was introduced in 2014, with the goal of being able to orchestrate infrastructure as code. It first targeted AWS but has grown to be able to manage a large ecosystem of modules.  In fact, the capability of multi-provider support is one of the main selling points of the technology. Terraform introduced its own DSL, called Hashicorp Configuration Language (HCL). On the surface, it feels like a more human-friendly JSON. JSON is also natively supported within Terraform if you have a masochistic side.

#### CloudFormation

CloudFormation (CFN) is the original IaC tool for AWS, released in 2011. I have come to respect, hate, love, and revere its power to describe and manage infrastructure. CFN was originally only offered in JSON, but we were finally treated to a heaping helping of tabs vs spaces actually mattering with [native CFN YAML support in 2016](https://www.trek10.com/blog/cloudformation-yaml-and-why-its-awesome).

##### CloudFormation: Introduction

CloudFormation is one of the safest ways to build, manage, change, and destroy resources in your infrastructure. It offers robust resource state management, and these days it can tell you what is going to [happen before you run your deployment](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html).

A lot of great features have worked to make CloudFormation more enjoyable or productive to work with over the years.

**CloudFormation Macros & Transforms**

One of the more powerful concepts, bringing whole new capabilities to essentially add your own opinionated capabilities to CloudFormation. For example, Trek10 provides a macro that lets you [write Step Function Amazon States Language (ASL) with pure yaml](https://github.com/trek10inc/sfn-yaml-macro) and smartly resolves CFN intrinsic functions.

You could imagine being able to provide opinionated IAM policy generators or S3 bucket resource macros. Whatever you want to do, macros can likely get you there. Take note, while powerful, you are treading dangerous territory as it becomes easy to effectively build your own Domain Specific Language. Instead of CloudFormation managing your resources, you are using CloudFormation as a bad Domain-Specific Language compiler that you have to babysit.

**Resource Providers**

For a while, we only had Custom Resources to provision and manage resources that CloudFormation didn’t natively support. This is now largely superseded by Resource Providers which allow you to create private or published providers to bring the management of third party and unsupported resources into your stacks. For example, Datadog, a popular monitoring tool can be used in your stack to provision and manage your monitoring without needing some out-of-band process.

___In most of my recent work with CFN, I’ve defaulted to using the AWS Serverless Application Model, or [SAM](https://aws.amazon.com/serverless/sam/). SAM is a superset of CFN, with some handy transformations that let you do a bit less typing and wiring up of various resources and permissions. Think of it like a well thought out and “managed” macro. If you are doing anything with AWS Lambda or event-driven computing and looking to level up your YAML wrangling, start with SAM.___

#### Terraform

Terraform was introduced in 2014, with the goal of being able to orchestrate infrastructure as code. It first [targeted AWS](https://github.com/hashicorp/terraform/commit/d6d5a97ec9cd08658e015ca83b34da3795e199ad) but has grown to be able to manage a [large ecosystem of modules](https://registry.terraform.io/).  In fact, the capability of multi-provider support is one of the main selling points of the technology.
  
##### Terraform: Introduction

**HCL**

Terraform introduced its own DSL, called [Hashicorp Configuration Language](https://github.com/hashicorp/hcl) (HCL). On the surface, it feels like a more human-friendly JSON. JSON is also natively supported within Terraform if you have a masochistic side.

> HCL is a powerful configuration language that helps use Terraform to the highest potential!
>

**Terraform vs CloudFormation**

AWS Infrastructure as Code is just fancy state management. The biggest difference between Terraform and CloudFormation is how it actually interacts with the infrastructure itself. CloudFormation you can hand a representation of your goal state, and it will perform all the operations on your infrastructure to get there for you natively within the platform. Terraform likewise takes the representation of your goal state, and [constructs a plan](https://www.terraform.io/docs/commands/plan.html) of API calls directly to your AWS infrastructure to get to that state.

**Why use Terraform over CloudFormation?**

In a perfect world, both approaches work flawlessly. But this is the cloud we are talking about. [Everything fails all the time](https://www.allthingsdistributed.com/2016/03/10-lessons-from-10-years-of-aws.html) as Werner Vogels says. Until recently, Terraform was superior in terms of being able to recover from people going outside the process to update resources.

Terraform was able to resolve inconsistencies and refresh a correct state of the infrastructure even if someone had manually edited that security group “just to test something”. CloudFormation struggled with these inconsistent states, but the introduction of [Drift Detection](https://aws.amazon.com/blogs/aws/new-cloudformation-drift-detection/) attempted to solve some of this headache.

**Even if you didn’t start with IaC, you can move to it**

Terraform also still offers the more elegant story of [importing](https://www.terraform.io/docs/import/index.html) unmanaged resources, or resources from other stacks. CloudFormation offers this, but only for the subset of resources that support drift detection.

In addition to these benefits, Terraform on AWS is really the one true option for “learn once, utilize most places”. Regardless of your feelings on multi-cloud or hybrid-cloud, the appeal of training up yourself or staff on a singular technology that can benefit from knowledge transfer across many different possible targets is tempting.

#### CDK

AWS Cloud Development Kit (CDK) is the new kid on the block, released in 2019. Using familiar programming languages and provided libraries in TypeScript, Python, Java and .NET developers can write with the same code as the rest of their stack to manage their infrastructure.

##### CDK: Introduction

CDK, however, is not devoid of CloudFormation. In fact, CDK synthesizes to CloudFormation. You still leverage all the state management and inherent benefits (and downsides) of CloudFormation by adopting CDK.

A quick aside: I do want to highlight that some folks view CFN as the “assembly language” of AWS, largely because of how many tools “compile” down to CFN. I think this is a dangerous comparison. It can lead to the interpretation that, like any high-level language to assembly, you don’t really need to understand how the lower-level instruction set works to effectively leverage the higher-level constructs. In my experience, this is patently untrue in the case of CFN. Even a rudimentary understanding of CFN leads to better decisions in the higher level usages like CDK.

Ultimately, I would contend that **CDK is the most comfortable and natural entry point for developers to start building Cloud Native applications**. 

**Constructs**

One of the particularly powerful features of CDK that I believe CloudFormation has struggled to natively deliver is the idea of truly shareable and reusable modules. CDK has introduced the concept of [constructs](https://docs.aws.amazon.com/cdk/latest/guide/constructs.html).
Constructs in practice provide everything from simple wrappings of some specific defaults you would like to re-use across your project all the way to complex multi-resource orchestration and wrapping of [resource providers](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/resource-types.html).
The distribution method for these constructs then relies on the native

The other important part of CDK Constructs is something neat called [jsii](https://github.com/aws/jsii).
To quote the project; “jsii allows code in any language to naturally interact with JavaScript classes. It is the technology that enables the AWS Cloud Development Kit to deliver polyglot libraries from a single codebase!”.
If you write your constructs with TypeScript, it is fairly straight forward to distribute and utilize those constructs across the other core CDK languages – further encouraging sharing of modules.

One of the most elegant ways I can illustrate how nice the CDK experience can be is to put a side-by-side comparison of the usage of [Amazon States Language](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-amazon-states-language.html) (ASL).

First what it looks like in CloudFormation Native ASL:

```json
{
  "DeliveryStepFunctionStateMachine": {
    "Type": "AWS::StepFunctions::StateMachine",
    "Properties": {
      "RoleArn": {
        "Fn::GetAtt": ["DeliveryStepFunctionStateMachineRoleC6479370", "Arn"]
      },
      "DefinitionString": {
        "Fn::Join": [
          "",
          [
            "{\"StartAt\":\"MapperTask\",\"States\":{\"MapperTask\":{\"Next\":\"SetStatusTo-pending\",\"Retry\":[{\"ErrorEquals\":[\"States.ALL\"],\"MaxAttempts\":10}],\"Parameters\":{\"FunctionName\":\"",
            {
              "Ref": "DeliveryStepFunctionMapper"
            },
            "\",\"Payload.$\":\"$\"},\"OutputPath\":\"$.Payload\",\"Type\":\"Task\",\"Resource\":\"arn:",
            {
              "Ref": "AWS::Partition"
            },
            ":states:::lambda:invoke\"},\"SetStatusTo-pending\":{\"Next\":\"retry seconds\",\"Type\":\"Task\",\"ResultPath\":null,\"Resource\":\"arn:",
            {
              "Ref": "AWS::Partition"
            },
            ":states:::dynamodb:updateItem\",\"Parameters\":{\"Key\":{\"pk\":{\"S.$\":\"$.pk\"},\"sk\":{\"S.$\":\"$.sk\"}},\"TableName\":\"",
            {
              "Ref": "PersistenceDDBTable"
            },
            "\",\"ExpressionAttributeNames\":{\"#status\":\"status\"},\"ExpressionAttributeValues\":{\":status\":{\"S\":\"pending\"}},\"ReturnValues\":\"ALL_NEW\",\"UpdateExpression\":\"SET #status = :status\"}},\"retry seconds\":{\"Type\":\"Wait\",\"SecondsPath\":\"$.retrySeconds\",\"Next\":\"SetStatusTo-in-progress\"},\"SetStatusTo-in-progress\":{\"Next\":\"DeliverTransactionTask\",\"Type\":\"Task\",\"ResultPath\":null,\"Resource\":\"arn:",
            {
              "Ref": "AWS::Partition"
            },
            ":states:::dynamodb:updateItem\",\"Parameters\":{\"Key\":{\"pk\":{\"S.$\":\"$.pk\"},\"sk\":{\"S.$\":\"$.sk\"}},\"TableName\":\"",
            {
              "Ref": "PersistenceDDBTable"
            },
            "\",\"ExpressionAttributeNames\":{\"#status\":\"status\"},\"ExpressionAttributeValues\":{\":status\":{\"S\":\"in-progress\"}},\"ReturnValues\":\"ALL_NEW\",\"UpdateExpression\":\"SET #status = :status\"}},\"DeliverTransactionTask\":{\"Next\":\"Delivery success?\",\"Retry\":[{\"ErrorEquals\":[\"States.ALL\"],\"MaxAttempts\":10}],\"Parameters\":{\"FunctionName\":\"",
            {
              "Ref": "DeliveryStepFunctionDeliverTransaction"
            },
            "\",\"Payload.$\":\"$\"},\"OutputPath\":\"$.Payload\",\"Type\":\"Task\",\"Resource\":\"arn:",
            {
              "Ref": "AWS::Partition"
            },
            ":states:::lambda:invoke\"},\"Delivery success?\":{\"Type\":\"Choice\",\"Choices\":[{\"Variable\":\"$.status\",\"StringEquals\":\"complete\",\"Next\":\"SetStatusTo-complete\"},{\"Variable\":\"$.status\",\"StringEquals\":\"failed\",\"Next\":\"SetStatusTo-failed\"}],\"Default\":\"SetStatusTo-pending\"},\"SetStatusTo-complete\":{\"End\":true,\"Type\":\"Task\",\"ResultPath\":null,\"Resource\":\"arn:",
            {
              "Ref": "AWS::Partition"
            },
            ":states:::dynamodb:updateItem\",\"Parameters\":{\"Key\":{\"pk\":{\"S.$\":\"$.pk\"},\"sk\":{\"S.$\":\"$.sk\"}},\"TableName\":\"",
            {
              "Ref": "PersistenceDDBTable"
            },
            "\",\"ExpressionAttributeNames\":{\"#status\":\"status\"},\"ExpressionAttributeValues\":{\":status\":{\"S\":\"complete\"}},\"ReturnValues\":\"ALL_NEW\",\"UpdateExpression\":\"SET #status = :status\"}},\"SetStatusTo-failed\":{\"End\":true,\"Type\":\"Task\",\"ResultPath\":null,\"Resource\":\"arn:",
            {
              "Ref": "AWS::Partition"
            },
            ":states:::dynamodb:updateItem\",\"Parameters\":{\"Key\":{\"pk\":{\"S.$\":\"$.pk\"},\"sk\":{\"S.$\":\"$.sk\"}},\"TableName\":\"",
            {
              "Ref": "PersistenceDDBTable"
            },
            "\",\"ExpressionAttributeNames\":{\"#status\":\"status\"},\"ExpressionAttributeValues\":{\":status\":{\"S\":\"failed\"}},\"ReturnValues\":\"ALL_NEW\",\"UpdateExpression\":\"SET #status = :status\"}}}}"
          ]
        ]
      }
    }
  }
}
```

Then with AWS CDK (leveraging some existing constructs to handle editing the Amazon DynamoDB records for me):

```TypeScript
const STATUS = "$.status"
const RETRY_SECONDS = "$.retrySeconds"
const PENDING = "pending"
const PROGRESS = "in-progress"
const FAILED = "failed"
const COMPLETE = "complete"

const setPending = stepFunction.setStatus(this, props.table, PENDING);
const setProgress = stepFunction.setStatus(this, props.table, PROGRESS);
const setSuccess = stepFunction.setStatus(this, props.table, COMPLETE);
const setFailed = stepFunction.setStatus(this, props.table, FAILED);
const waitForNSeconds = this.waitTask("retry seconds", RETRY_SECONDS);

const definition = this.mapperTask()
  .next(setPending)
  .next(waitForNSeconds)
  .next(setProgress)
  .next(this.deliverTransactionTask())
  .next(
    new sfn.Choice(this, "Delivery success?")
      .when(sfn.Condition.stringEquals(STATUS, COMPLETE), setComplete)
      .when(sfn.Condition.stringEquals(STATUS, FAILED), setFailed)
      .otherwise(setPending)
  );
```

If you had to read the second code snippet to understand what the first was doing, I’d completely understand. Granted, there is nothing stopping CloudFormation from adopting and supporting a more elegant DSL.
In fact, [AWS SAM](https://aws.amazon.com/serverless/sam/) is really an attempt at exactly this with a focus on the serverless developer experience. 

Given the current community momentum around CDK and growing investment from AWS, I expect to see more and more teams starting with CDK and happily continuing with it as their primary utility for infrastructure management.

#### Terraform vs CloudFormation vs AWS CDK

| Am I working on a simple, mostly serverless solution with minimal dependency or dependents? | CloudFormation (particularly AWS SAM) is likely enough. |
| Do I have a top-down distribution of best practices and orchestration?	                  | CDK or Terraform                                        |
| Do I want to stay entirely within the AWS ecosystem?	                                      | CloudFormation or CDK                                   |
| Do I need to orchestrate resources outside the AWS ecosystem?	                              | Terraform or CDK for Terraform                          |
| Do I want a multi-provider utility, especially for multi/hybrid cloud knowledge transfer?	  | Terraform                                               |

Choosing the right IaC tool on AWS

The only truly wrong answer is the one that prevents you from building anything at all.

#### Other IaC tools and Terraform alternatives

The IaC space is growing, everyone has their own opinion and how things should work.
I’d argue competition is healthy and in some cases has forced the providers themselves to step their game up.

| AWS Amplify CLI	   | A CLI toolchain for simplifying serverless web and mobile development. If you are primarily a frontend developer, or just want to get going as fast as possible, look no further. The Amplify CLI and framework manages all the complexity behind the scenes to help you build and deploy real-time web and mobile applications. |
| Pulumi	           | If the Terraform and CDK teams got together and reimagined things, I get the sense it would look a bit like Pulumi.                                                                                                                                                                                                              |
| Troposphere	       | The troposphere library allows for easier creation of the AWS CloudFormation JSON by writing Python code to describe the AWS resources. Troposphere also includes some basic support for OpenStack resources via Heat.                                                                                                           |
| InGraph	           | InGraph is an open-source and declarative, infrastructure graph DSL for AWS CloudFormation. The key feature is the ability to create composable infrastructure components while preserving the rigorous semantic of the AWS CloudFormation language.                                                                             |
| Serverless Framework | Zero-friction serverless development. Easily build apps that auto-scale on low cost, next-gen cloud infrastructure.                                                                                                                                                                                                              |

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
