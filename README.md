# IOpipe to New Relic Serverless Migration Reference Guide
Function faster on AWS Lambda with New Relic Serverless

Includes:
* Overview
* Adding New Relic Observability with Lambda Layers
* FAQ

-----


## Overview:

If you’re currently using IOpipe to monitor and debug your AWS Lambda applications, our goal is to make the transition to New Relic Serverless as simple and efficient as possible. 

We’ve built a New Relic Lambda Layer with auto-instrumented observability with install instructions below [jumplink to section].

In addition, we’re providing a free trial of New Relic Serverless and extensive support through our Community Slack. 

If you have a question that’s not answered in our FAQs or a feature request for our roadmap, please reach out via Slack.

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
* NR Insights Dashboard


## Install Instructions

Enable New Relic Monitoring Requirements:  https://docs.newrelic.com/docs/serverless-function-monitoring/aws-lambda-monitoring/get-started/enable-new-relic-monitoring-aws-lambda

This requires you to have the AWS CLI setup.

## Recommended method: 
Updating Lambda Layers for Serverless Framework users

Serverless-newrelic-layers
A Serverless plugin to add New Relic observability using AWS Lambda Layers without requiring a code change.

### Requirements
serverless >= 1.34.0

### Features
* Supports Node.js and Python runtimes (more runtimes to come)
* No code change required to enable New Relic
* Bundles New Relic's agent in a single layer

### Install
With NPM:

```
npm install --save-dev serverless-newrelic-layers
```

With yarn:

```
yarn add --dev serverless-newrelic-layers
```

Add the plugin to your serverless.yml:

```
plugins:
  - serverless-newrelic-layers
  ```
  
Get a New Relic license key and plug it into your serverless.yml:

```
custom:
  newRelic:
      accountId: your-new-relic-account-id-here
```

Deploy and you're all set.

### Usage
This plugin wraps your handlers without requiring a code change. If you're currently using a New Relic agent, you can remove the wrapping code you currently have, and this plugin will do it for you automatically.

### Config
The following config options are available via the newRelic section of the custom section of your serverless.yml:

### `licenseKey` (required)
The New Relic license key to use.

### `debug` (optional)
Whether or not to enable debug mode. Must be a boolean value.

```
custom:
  newRelic:
    debug: true
```
    
### `exclude` (optional)
An array of functions to exclude from automatic wrapping.

```
custom:
  newRelic:
    exclude:
      - excluded-func-1
      - another-excluded-func
```

### `layer_arn` (optional)
Pin to a specific layer version. The latest layer ARN is automatically fetched from the New Relic Layers API

### `prepend` (optional)
Whether or not to prepend the IOpipe layer. Defaults to false which appends the layer.

```
custom:
  newRelic:
    prepend: true
```

### Supported Runtimes
This plugin currently supports the following AWS runtimes:

* nodejs8.10
* nodejs10.x
* python2.7
* python3.6
* python3.7

## Manual Layer Install
Find the supporting layer for your runtime and region: https://us-east-1.nr-layers.iopipe.com/get-layers
Example request: $ curl https://us-east-1.nr-layers.iopipe.com/get-layers?CompatibleRuntime=python3.7  

Grab the ARN of the most recent version and add it in the AWS Lambda console for your function

Update your functions handler to point to the newly attached layer in the console for your function

Python: `newrelic_lambda_wrapper.handler`
Node: `newrelic-lambda-wrapper.handler`

Environment Variables
`NEW_RELIC_ACCOUNT_ID`: Your NewRelic Account ID
`NEW_RELIC_LAMBDA_HANDLER`: Path to your initial handler

## Migration FAQ

### Do I have to set up a new NR account?
If you are not already a New Relic customer, you will need to sign up for a New Relic account before installing the Serverless plugin.

### How much time will I have to migrate to New Relic Serverless?
You will have 30 days from Nov. 1, 2019 to make the migration to New Relic. We are publishing a series of documentation available via email, in-app, Slack, and meetings to support you in the transition. 

Do not hesitate to reach out if you have a question. We are here to help every step of the way.

### What if I’m already a New Relic customer?
Good news! You can add New Relic Serverless monitoring to your AWS Lambda functions simply by updating to the New Relic Layer provided by the IOpipe team. 

See the New Relic Layer install instructions above.

### Will IOpipe users get any discount on New Relic?
Yes. As part of the transition, we are offering a 30-day free trial of New Relic Serverless for Island customers to assist with onboarding and promote retention.

### Are there new features available on NR that were not on Island?
The New Relic agent provides additional auto-tracing capabilities out of the box, including numerous database technologies.

New Relic’s interface allows users to create their own custom dashboards.

### How do I migrate my custom instrumentation to New Relic?
To configure custom instrumentation in New Relic, the following documentation will get you started. 

NodeJS: https://docs.newrelic.com/docs/agents/nodejs-agent/api-guides/nodejs-agent-api

Python: https://docs.newrelic.com/docs/agents/python-agent/python-agent-api

New Relic receives data in two primary forms:
#### 1. Metric data measures numeric, time-based values; for example, connections per minute.

NR Custom Metric API calls: 
Python
```
newrelic.agent.record_custom_metric(name, value, application=None) 
```

NodeJS
```
newrelic.recordMetric(name, value)
``` 
#### 2. Event data captures discrete event information. 
Events have key-value attributes attached to them. You can analyze and query event data in New Relic Insights as well as on the individual invocation record:

NR Custom Event API calls:
Python
```
newrelic.agent.record_custom_event(event_type, params, application=None)
```

Node.js
```
newrelic.recordCustomEvent(eventType, attributes) 
```

Python Example
```
import newrelic

def hello(event, context):
    newrelic.agent.record_custom_event('CustomEvent', {'foo': 'bar'})
```

Node Example
```
'use strict';
const newrelic = require('newrelic');
require('@newrelic/aws-sdk');

module.exports.hello = async event => {
  newrelic.recordCustomEvent("MyEventType", {foo: "bar"});
};
```
### What can New Relic Serverless users look forward to? 
Right out of the gate, customers will be able to get critical monitoring, troubleshooting, alerting, and visualization of the behaviors of their AWS Lambda functions with New Relic Serverless. 

Our team will initially be focused on integrating key technology, like our support of AWS Lambda Layers and popular deployment frameworks, into the New Relic solution. This should substantially reduce onboarding friction, so we can help our customers get the great benefits of the New Relic platform even sooner. Our team will then focus on serverless roadmap acceleration as New Relic looks forward to supporting other FaaS and serverless technologies and improving the overall developer experience.

If you have something specific in mind, we want to hear from you. Reach out via our Community Slack.




