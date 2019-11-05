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

#### Features:

* Metrics
* Tracing 
* Logging
* Alerts
* Drilldown Search
* Error Aggregation
* New Relic Insights Dashboard


## Install Instructions

### Enable New Relic monitoring of AWS Lambda Cloudwatch Logs

New Relic monitoring for AWS Lambda offers in-depth performance monitoring for your Lambda functions. This document explains how to enable this feature and get started using it.

This requires you to have the AWS CLI setup.

## Step 1: Configure AWS to communicate with New Relic
In this section, you'll run a set-up script that does the following:

- Configures your AWS account and Lambda function to communicate with New Relic.
- Configures a New Relic log-ingestion Lambda that will send your Lambda log data to New Relic.

To use the script:
1. Ensure you've downloaded [the script](https://docs.newrelic.com/docs/serverless-function-monitoring/aws-lambda-monitoring/get-started/enable-new-relic-monitoring-aws-lambda#script) and meet its requirements.
2. Optional: If you have multiple AWS profiles and don't want to use the default, use `AWS_DEFAULT_PROFILE`[environment variable](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) to set another profile name. Ensure the profile is properly configured (including the default region). Example:

```
export AWS_DEFAULT_PROFILE=MY_OTHER_PROFILE
```
3. Run the following command in the same directory where the script is located:
```
./newrelic-cloud set-up-lambda-integration --nr-account-id YOUR_ACCOUNT_ID \
--linked-account-name YOUR_LINKED_ACCOUNT_NAME \
--nr-api-key YOUR_NR_API_KEY
```

Notes:

- This defaults to US. If you are integrating with the New Relic EU region, add the following argument:

```
--nr-region "eu"
```
- The value of your `YOUR_NR_API_KEY` is your Personal API Key. This is not the same as your New Relic REST API key. For more information about `YOUR_NR_API_KEY` and other arguments, see [New Relic's Lambda documentation on GitHub](https://github.com/newrelic/nr-lambda-onboarding#arguments)  .

4. Optional: If you want to stream all logs to New Relic Logs:
   1. Go to the New Relic `newrelic-log-ingestion` Lambda and set the `LOGGING_ENABLED` environment variable to `true`
   2. Remove the `NR_LAMBDA_MONITORING` Subscription filter pattern. Go to the Log Group for each monitored Lambda, remove the `newrelic-log-ingestion` subscription and re-add it back. (There is no way to edit existing filter patterns).

For more manual alternatives to using the script, or to learn what actions it performs, see the [manual instructions](https://docs.newrelic.com/docs/serverless-function-monitoring/aws-lambda-monitoring/get-started/enable-new-relic-monitoring-aws-lambda#manual-setup).


## Step 2a: Instrument Lambda Code using Serverless Framework Plugin (recommended method): 

Updating Lambda Layers for Serverless Framework users

[serverless-newrelic-lambda-layers](https://github.com/iopipe/serverless-newrelic-lambda-layers)
A Serverless plugin to add [New Relic observability](https://newrelic.com/products/serverless-aws-lambda) using AWS Lambda Layers without requiring a code change.

### Requirements

* [serverless](https://github.com/serverless/serverless/) >= 1.34.0
* Set up the [New Relic AWS Integration](https://docs.newrelic.com/docs/serverless-function-monitoring/aws-lambda-monitoring/get-started/enable-new-relic-monitoring-aws-lambda#enable-process) (only the `set-up-lambda-integration` step is required)

### Features

* Supports Node.js and Python runtimes (more runtimes to come)
* No code change required to enable New Relic
* Bundles New Relic's agent in a single layer
* Configures CloudWatch subscription filters automatically

### Install

With NPM:

```bash
npm install --save-dev serverless-newrelic-lambda-layers
```

With yarn:

```bash
yarn add --dev serverless-newrelic-lambda-layers
```

Add the plugin to your serverless.yml:

```yaml
plugins:
  - serverless-newrelic-lambda-layers
```

If you don't yet have a New Relic account, [sign up here](https://newrelic.com/products/serverless-aws-lambda).

Then set up the [New Relic AWS Integration](https://docs.newrelic.com/docs/serverless-function-monitoring/aws-lambda-monitoring/get-started/enable-new-relic-monitoring-aws-lambda#enable-process) (only the `set-up-lambda-integration` step is required).

Get your [New Relic Account ID](https://docs.newrelic.com/docs/accounts/install-new-relic/account-setup/account-id) and plug it into your `serverless.yml`:

```yaml
custom:
  newRelic:
      accountId: your-new-relic-account-id-here
```

Deploy:

```bash
sls deploy
```

And you're all set.

### Usage

This plugin wraps your handlers without requiring a code change. If you're currently using a New Relic agent, you can remove the wrapping code you currently have, and this plugin will do it for you automatically.

For additional configuration options check out the [README](https://github.com/iopipe/serverless-newrelic-lambda-layers/).

For more on how to instrument your functions:

* [Node.js Instrumentation Guide](https://docs.newrelic.com/docs/agents/nodejs-agent/getting-started/introduction-new-relic-nodejs#extend-instrumentation)
* [Python Instrumentation Guide](https://docs.newrelic.com/docs/agents/python-agent/custom-instrumentation/python-custom-instrumentation)

### Supported Runtimes

This plugin currently supports the following AWS runtimes:

* nodejs8.10
* nodejs10.x
* python2.7
* python3.6
* python3.7

## Step 2b:  Instrument Lambda Code using the Manual Layer Install method

Find the supporting layer for your runtime and region [here](https://nr-layers.iopipe.com).

Grab the ARN of the most recent version and add it in the AWS Lambda console for your function

Update your functions handler to point to the newly attached layer in the console for your function

Python: `newrelic_lambda_wrapper.handler`
Node: `newrelic-lambda-wrapper.handler`

Set the following Environment Variables

`NEW_RELIC_ACCOUNT_ID`: Your NewRelic [Account ID](https://docs.newrelic.com/docs/accounts/install-new-relic/account-setup/account-id)
`NEW_RELIC_LAMBDA_HANDLER`: Path to your initial handler

## Migration FAQ

### Do I have to set up a new New Relic account?

If you are not already a New Relic customer, you will need to [sign up](https://newrelic.com/signup) for a New Relic account before installing the Serverless plugin.

### How much time will I have to migrate to New Relic Serverless?

You will have 30 days from Nov. 1, 2019 to make the migration to New Relic. We are publishing a series of documentation available via email, in-app, Slack, and meetings to support you in the transition.

Do not hesitate to [reach out](https://iopipe.now.sh/) if you have a question. We are here to help every step of the way.

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

