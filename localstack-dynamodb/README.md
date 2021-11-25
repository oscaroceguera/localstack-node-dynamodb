**Create docker-compose.yml**

```yml
version: '3.0'

services:

  localstack:
    image: localstack/localstack:latest
    environment: 
      - AWS_DEFAULT_REGION=ap-southeast-1
      - EDGE_PORT=4566
      - SERVICES=dynamodb
      - KINESIS_PROVIDER=kinesalite
    ports:
      - '4566:4566'
    volumes:
      - "${TMPDIR:-/tmp/localstack}:/tmp/localstack"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

`docker-compose up`


**Create Table**

```shell
aws dynamodb --endpoint-url=http://localhost:4566 create-table \
    --table-name Music \
    --attribute-definitions \
        AttributeName=Artist,AttributeType=S \
        AttributeName=SongTitle,AttributeType=S \
    --key-schema \
        AttributeName=Artist,KeyType=HASH \
        AttributeName=SongTitle,KeyType=RANGE \
--provisioned-throughput \
        ReadCapacityUnits=10,WriteCapacityUnits=5
```

**Check TABLE STATUS**

```shell
aws --endpoint-url=http://localhost:4566 dynamodb describe-table --table-name Music | grep TableStatus
```

**INSERT DATA To TABLE**

```shell
aws --endpoint-url=http://localhost:4566 dynamodb put-item \
    --table-name Music  \
    --item \
        '{"Artist": {"S": "No One You Know"}, "SongTitle": {"S": "Call Me Today"}, "AlbumTitle": {"S": "Somewhat Famous"}, "Awards": {"N": "1"}}'
```

**Read Table**

```shell
aws dynamodb scan --endpoint-url=http://localhost:4566 --table-name Music
```

Reference: https://onexlab-io.medium.com/localstack-dynamodb-8befdaac802b

