- Run docker-compose.yml file
```shell
docker-compose up
```

- Add temp credentials in a other shell
```shell
aws configure

// output
AWS Access Key ID [****************temp]: temp
AWS Secret Access Key [****************temp]: temp
Default region name [us-west-2]: 
Default output format [json]: 

```

- Create s3 butcket
```shell
aws --endpoint-url=http://localhost:4566/ s3 mb s3://mostly-code-locala-butcket

// output
make_bucket: mostly-code-locala-butcket
```

- List s3 butcket
```shell
aws --endpoint-url=http://localhost:4566/ s3 ls

// output
2021-11-25 18:08:20 mostly-code-locala-butcket
```

- Create test-file.txt

- Updload the file into the butcket
```shell
aws s3 cp test-file.txt s3://mostly-code-locala-butcket/test-file.txt --endpoint=http://localhost:4566/

// output
upload: ./test-file.txt to s3://mostly-code-locala-butcket/test-file.txt
```

- Create dynamodb-template.json file to create a table
```json
{
  "TableName": "demo-table-creation-localstack",
  "AttributeDefinitions": [{"AttributeName": "ID", "AttributeType": "S"}],
  "KeySchema": [
    {
      "AttributeName": "ID",
      "KeyType": "HASH"
    }
  ],
  "ProvisionedThroughput": {
    "ReadCapacityUnits": 5,
    "WriteCapacityUnits": 5
  }
}
```

- Create table by commnand using the dynamodb-template.json
```shell
aws --endpoint-ur=http://localhost:4566/ dynamodb create-table --cli-input-json file:///Users/oscar.oceguera/.../mock-aws-s3-dynamodb/dynamodb-template.json
```

- List Tables
```shell
aws --endpoint-ur=http://localhost:4566/ dynamodb list-tables
```

- Delete Table
```shell
aws --endpoint-ur=http://localhost:4566/ dynamodb delete-table --table-name demo-table-creation-localstack
```
