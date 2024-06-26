# LocalStack Demo

Simple demo to provision AWS Lambda, APIGateway through Terraform and LocalStack

## Why i make this demo
1st, it's always nice for developers to be able to have a local dev env to test before shipping to the AWS cloud.
2nd reason is AWS will bill you for the APIgateway, so if you only want to play around, by refer to my doc, it will cost you nothing at all. As everything is running on your own computer.

## Prerequisites

* [LocalStack](https://docs.localstack.cloud/getting-started/installation/)
* [Docker](https://docs.docker.com/desktop/install/mac-install/)
* [AWS Cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* [Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli#install-terraform)
* [tfenv](https://github.com/tfutils/tfenv) 


## Running LocalStack

Use the `localstack` CLI command to get your local AWS env started:
```
localstack start -d
# setup aws profile for localstack https://docs.localstack.cloud/user-guide/integrations/aws-cli/#aws-cli
# ~/.aws/config
[profile localstack]
region=us-east-1
output=json
endpoint_url = http://localhost:4566

# ~/.aws/credentials
[localstack]
aws_access_key_id=test
aws_secret_access_key=test
```


## Git clone
```bash
git clone https://github.com/ShengzhenFu/go-lambda-apigateway-terraform.git
cd go-lambda-apigateway-terraform
```

## Deploy Lambda & APIGateway
```bash
tfenv install 1.7.4
tfenv use 1.7.4
echo 1.7.4 > .terraform-version
make deploy
```

This will deploy the AWS ApiGateway integrated with Lambda (Golang) via Terraform. You can check `Makefile` for more details

If you see the outputs like below ... :point_down: Congratulations, deploy is successful :thumbsup: 
![output](./docs/output.png "terraform, output")

## Testing
Let's test the API endpoint. 
Run below command (replace the apigateway_id with the output you got above)
```bash
curl -X POST "http://{apigateway_id}.execute-api.localhost.localstack.cloud:4566/local/api?name=Localstack" -H "content-type: application/json"
```

If your result is like below, then congratulations ! it works as expected.

![test](./docs/test.png "apigateway test aws")


you could also use tools like POSTMAN to test it. 
![postman](./docs/postman.png "postman test api")


## Cleanup
```bash
make destroy
localstack stop
```

### TODO
- CDK implementation
- CI/CD CodePipeline
- Make a more practical demo of Golang project 
