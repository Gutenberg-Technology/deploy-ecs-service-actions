# deploy-ecs-service-actions
Composite actions to deploy a new version of an ECS service

## Overview

This Composite actions will:
- Render a new task definition revision
- Optionally replace `__AWS_ACCOUNT__` and `__ENV__` in the task definition to match AWS account & environment target
- Update the ECS service with the new task definition revision

## How to use

```yaml
jobs:
  deploy-ecs-td:
      name: render & deploy new task definition
      runs-on: ubuntu-latest
      steps:
        - name: Deploy ECS Service
            uses: Gutenberg-Technology/deploy-ecs-service-actions@v1.0.0
            with:
                env: "rct"
                cluster: "my-cluster"
                service: "my-service"
                task-def-json: task_def_tpl.json
                container-name: "my-container"
                tagged-image: "my-image:tag-123456"
                aws-access-key-id: 123456789
                aws-secret-access-key: 123456789
                aws-region: "us-east-1"
```

## Customize AWS account & env

In task definition, you can optionally replace `__AWS_ACCOUNT__`, `__AWS_REGION__`and `__ENV__` with a value from the `env` input and according to the AWS account.

Example:
```json
{
    "containerDefinitions": [
        {
            "name": "my-container",
            "image": "my-image:__ENV__"
        },
        # [...]
        "arn": "arn:aws:ssm:__AWS_REGION__:__AWS_ACCOUNT__:parameter/abn-__ENV__/keycloak-mef-admin-user",
    ]
}
```

With `inputs.env = 'prod'`, `inputs.aws-region = 'eu-west-1'` and an AWS account called `123456798` it will be rendered as:

```json
{
    "containerDefinitions": [
        {
            "name": "my-container",
            "image": "my-image:prod"
        },
        # [...]
        "arn": "arn:aws:ssm:eu-west-1:123456789:parameter/abn-__ENV__/keycloak-mef-admin-user"
    ]
}
```