- `npm install -g serverless`

- `serverless create --template aws-nodejs --path localstack-lamda`

- create docker-compose.yml
```yml
version: '3.1'

services:
  localstack:
    image: localstack/localstack:latest
    environment:
      - AWS_DEFAULT_REGION=us-east-1
      - EDGE_PORT=4566
      - SERVICES=lambda,s3,cloudformation,sts
    ports:
      - '4566-4597:4566-4597'
    volumes:
      - "${TEMPDIIR:-/tmp/localstack}:/temp/localstack"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

- `http://localhost:4566/health`

- localstack serveless plugin `npm install --save-dev serverless-localstack`

- Add plugin within serverless.yml
```yml
service: localstack-lamda
plugins:
  - serverless-localstack

custom:
  localstack:
    debug: true
    stages:
    - local
    - dev
    endpointFile: localstack_endpoints.json

frameworkVersion: "2"

provider:
  name: aws
  runtime: nodejs12.x
  lambdaHashingVersion: 20201221

functions:
  hello:
    handler: handler.hello

```

- create localstack_endpoints.json:
```json
{
  "CloudFormation": "http://localhost:4566",
  "CloudWatch": "http://localhost:4566",
  "Lambda": "http://localhost:4566",
  "S3": "http://localhost:4566"
}
```

- `serverless deploy --stage local`

- `serverless info --stage local`

- `serverless invoke local -f hello -l`