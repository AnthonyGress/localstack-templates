# Setting up Localstack

## Dependencies
- python
- pip
- docker
- AWS CLI
- awslocal (optional)


## Install localstack & awslocal
- Install localstack using `python3 -m pip install localstack`
- Install awslocal using `pip install awscli-local`

## Setting up your pro api key
The api key needs to be a global env variable in your cli environment
- set `- LOCALSTACK_API_KEY=${LOCALSTACK_API_KEY- }` under environment in your docker.compose.yml


## Why awslocal?
It shortens your repetitive commands. It is a a thin wrapper around the aws command line interface for use with LocalStack.

> Example

`aws --endpoint-url=http://localhost:4566 kinesis list-streams`

to

`awslocal kinesis list-streams`

alternatively you can set up a bash alias using the following command to achieve the same thing
> BASH alias

`alias awslocal="AWS_ACCESS_KEY_ID=test AWS_SECRET_ACCESS_KEY=test AWS_DEFAULT_REGION=${DEFAULT_REGION:-$AWS_DEFAULT_REGION}} aws --endpoint-url=http://${LOCALSTACK_HOST:-localhost}:4566"`

## Timestream CLI commands
- Create DB:
`awslocal timestream-write create-database --database-name testDB`
- Create Table:
 `awslocal timestream-write create-table --database-name testDB --table-name testTable`
- Write records:
`awslocal timestream-write write-records --database-name testDB --table-name testTable --records '[{"MeasureName":"cpu","MeasureValue":"60","TimeUnit":"SECONDS","Time":"1636986409"}]'`

- Query:
`awslocal timestream-query query --query-string "SELECT CREATE_TIME_SERIES(time, measure_value::double) as cpu FROM testDB.testTable WHERE measure_name='cpu'"`

Create SQS Queue in localstack

`aws --endpoint-url=http://localhost:4566 sqs create-queue --queue-name test-queue`

Run the following command to add a batch of messages to the Localstack SQS

`aws --endpoint-url=http://localhost:4566 sqs send-message-batch --queue-url http://localhost:4566/000000000000/test --entries '[{"Id":"Event1","MessageBody":"Fuel report for account 0001"},{"Id":"Event2","MessageBody":"Fuel report for account 0002"},{"Id":"Event3","MessageBody":"Fuel report for account 0003"},{"Id":"Event4","MessageBody":"Fuel report for account 0004"},{"Id":"Event5","MessageBody":"Fuel report for account 0005"}]'`

## Other Commands

List SQS queue

`aws --endpoint-url=http://localhost:4566 sqs list-queues`

SQS Add single

`aws --endpoint-url=http://localhost:4566 sqs send-message --queue-url http://localhost:4566/000000000000/test-queue --message-body 'Welcome to SQS queue'`

SQS Receive Single

`aws --endpoint-url=http://localhost:4566 sqs receive-message --queue-url http://localhost:4566/000000000000/test`

Delete SQS queue

`aws --endpoint-url=http://localhost:4566 sqs delete-queue --queue-url http://localhost:4566/000000000000/test`


