AWS CodePipeline Slack Notifications 
=======================

This bot will notify you of CodePipeline progress (using [CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html)).

We attempt to provide a unified summary, by pulling together multiple events, as well as information obtained by the API into a single message view.

![Build](build.gif)


## Launch

```console
sam build
sam deploy --guided
```


## Configuration / Customization

No configuration is necessary per pipeline.  As part of the CF Stack, we subscribe to all CodePipeline and CodeBuild events (using [CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html)).

Required parameters:

- `SlackOAuthAccessToken`
- `SlackOAuthAccessToken`
- `SlackChannel` (defaults to `builds`).
- `SlackBotName` (defaults to `PipelineBuildBot`).
- `SlackBotIcon` (defaults to `:robot_face:` 🤖 ).


Your Slack App requires the following Bot Token Scopes:

- channels:history
- channels:manage
- chat:write
- chat:write.customize
- chat:write.public
- groups:history
- links:write


## How it works

We utilize [CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) for CodePipline and CodeBuild to get notified of all status changes.

Using the notifications, as well as using the CodePipeline APIs, we are able to present a unified summary of your Pipeline and Build status.


### IAM permissions

As part of the deployment, we create an IAM policy for the bot lambda function of:

```
Policies:
  - AWSLambdaBasicExecutionRole
  - Version: '2012-10-17'
    Statement:
      - Effect: Allow
        Action:
          - 'codepipeline:Get*'
          - 'codepipeline:List*'
        Resource: '*'
      - Effect: Allow
        Action:
          - 'codebuild:Get*'
        Resource: '*'
```

So we can retrieve information about all pipelines and builds.  See [template.yml](https://github.com/clevertech/rwconnect-infra/blob/master/code-pipeline-slack/template.yml) for more detail.
