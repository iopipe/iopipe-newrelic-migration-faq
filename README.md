# IOpipe to New Relic Serverless Migration Reference Guide

Includes:

* Overview
* Adding New Relic Observability with Lambda Layers
* FAQ

-----

## Overview:

We recently joined the [New Relic Serverless](https://blog.newrelic.com/product-news/iopipe/) team!

If you’re currently using IOpipe to monitor and debug your AWS Lambda applications, our goal is to make the transition to New Relic Serverless as simple and efficient as possible. 

We've built a New Relic Lambda Layer with auto-instrumented observability with install instructions below.

In addition, we’re providing a free trial of New Relic Serverless and extensive support through our Community [Slack](https://iopipe.now.sh/).

If you have a question that’s not answered in our FAQs or a feature request for our roadmap, please reach out via [Slack](https://iopipe.now.sh/).

#### Languages supported:

* Node.js
* Python
* Java
* Go
* .Net

#### Features:

* Metrics
* Tracing 
* Logging
* Alerts
* Drilldown Search
* Error Aggregation
* New Relic Insights Dashboard
* Distributed Tracing
* Observability for all of your Serverless and non-serverless workloads


## Getting Started

### Enable New Relic monitoring of AWS Lambda Cloudwatch Logs

If you don't already have a New Relic account, you can sign up for a free trial [here](https://newrelic.com/signup?trial=nr+serverless).

Once your trial is setup, you can head over to the install instructions to get started instrumenting your Lambda Functions with zero code changes required: https://docs.newrelic.com/docs/serverless-function-monitoring/aws-lambda-monitoring/get-started/introduction-new-relic-monitoring-aws-lambda

## Migration FAQ

### Do I have to set up a new New Relic account?

If you are not already a New Relic customer, you will need to [sign up](https://newrelic.com/signup?trial=nr+serverless) for a New Relic account before installing the Serverless plugin.

### What if I’m already a New Relic customer?

Good news! You can add New Relic Serverless monitoring to your AWS Lambda functions simply by updating to the [New Relic Layer](https://github.com/iopipe/serverless-newrelic-lambda-layers) provided by the IOpipe team.

See the New Relic Layer install instructions above.

### Are there new features available on New Relic that were not on IOpipe?

The New Relic agent provides additional auto-tracing capabilities out of the box, including numerous database technologies.

New Relic allows users to gain observability for any application, not just for AWS Lambda based applications.

New Relic’s interface allows users to create their own custom dashboards.

### What can New Relic Serverless users look forward to? 

Right out of the gate, customers will be able to get critical monitoring, troubleshooting, alerting, and visualization of the behaviors of their AWS Lambda functions with New Relic Serverless. 

Our team will initially be focused on integrating key technology, like our support of AWS Lambda Layers and popular deployment frameworks, into the New Relic solution. This should substantially reduce onboarding friction, so we can help our customers get the great benefits of the New Relic platform even sooner. Our team will then focus on serverless roadmap acceleration as New Relic looks forward to supporting other FaaS and serverless technologies and improving the overall developer experience.

If you have something specific in mind, we want to hear from you. Reach out via our [Community Slack](https://iopipe.now.sh/).

