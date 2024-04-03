# deploy-ecs-service-actions
Composite actions to deploy a new version of an ECS service

## Overview

This Composite actions will:
- Render a new task definition revision
- Update the ECS service with the new task definition

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
                cluster: my-cluster
                service: my-service
                task-def-json: /path/to/my-task-definition.json
                container-name: my-container
                tagged-image: my-image:latest
                aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
                aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

## Customize AWS account & env

In task definition, you can optionally replace `__AWS_ACCOUNT__` and `__ENV__` with a value from the `env` input and according to the AWS account.

Example:
```json
{
    "containerDefinitions": [
        {
            "name": "my-container",
            "image": "my-image:__ENV__"
        }
    ]
}
```

With `inputs.env = 'prod'`, it will be rendered as:

```json
{
    "containerDefinitions": [
        {
            "name": "my-container",
            "image": "my-image:prod"
        }
    ]
}
```