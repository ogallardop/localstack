### Getting started

## Installation
1. Install the AWS CLI. Even though we aren't going to be working with "real" AWS, we'll use this to talk to our local docker containers.

2. Once the AWS CLI is installed, run aws configure to create some credentials. Even though we're talking to our "fake" local service, we still need credentials. You can enter real credentials, or dummy ones. Localstack requires that these details are present, but doesn't actually validate them.

AWS Access Key ID: OSCAROSFODNN7EXAMPLE
AWS Secret Access Key: wJalrXudnFEMI/K9MDENG/bPxRfyciEXAMPLEKEY
Default region name: us-west-2
Default output format: json


----
## SNS 
# List topics
docker-compose exec localstack awslocal sns list-topics

# create topic
docker-compose exec localstack awslocal sns create-topic --name fintec-webhook
```
{
    "TopicArn": "arn:aws:sns:us-east-1:000000000000:fintec-webhook"
}
```
# subscribe to topic
docker-compose exec localstack awslocal sns subscribe --topic-arn arn:aws:sns:us-east-1:000000000000:fintec-webhook --protocol http --notification-endpoint "http://localhost:8030/banking/accounts/fintech-systems-webhooks"

# Send message to topic
docker-compose exec localstack awslocal sns publish --topic-arn arn:aws:sns:us-east-1:000000000000:fintec-webhook --message "Hello World"

## SQS

```docker-compose exec localstack awslocal sqs create-queue --queue-name webhookQueue```

```docker-compose exec localstack awslocal sqs list-queues```

```docker-compose exec localstack awslocal sqs receive-message --queue-url http://localhost:4576/queue/test_queue --attribute-names All --message-attribute-names All --max-number-of-messages 10```