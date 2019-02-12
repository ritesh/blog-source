+++
categories = ["til", " newstuff"]
date = "2019-02-12T14:37:12+00:00"
draft = true
summary = "An (unorganised) dump of things I'm looking at these days"
title = "TIL"
url = "til"

+++
# Feb 2019

Never had to test AWS Lambda functions locally (test in production YOLO). While walking through the docs with a colleague, I found that the nifty way to test a SAM lambda function is to run:

`$ sam local start-lambda`

In the directory where you have your SAM `template.yml`. This is `LambdaInvoking` the function, as opposed to passing it an event via API Gateway. From the docs, write a quick script like this to then invoke the Lambda. 

    import boto3
    import botocore
    
    # Set "running_locally" flag if you are running the integration test locally
    running_locally = True
    
    if running_locally:
    
        # Create Lambda SDK client to connect to appropriate Lambda endpoint
        lambda_client = boto3.client('lambda',
            region_name="eu-west-1",
            endpoint_url="http://127.0.0.1:3001",
            use_ssl=False,
            verify=False,
            config=botocore.client.Config(
                signature_version=botocore.UNSIGNED,
                read_timeout=30,
                retries={'max_attempts': 0},
            )
        )
    else:
        lambda_client = boto3.client('lambda')
    
    
    # Invoke your Lambda function as you normally usually do. The function will run
    # locally if it is configured to do so
    response = lambda_client.invoke(FunctionName="HelloFunction")
    
    # Verify the response
    assert response == "Hello World"

This is from the docs, but it took me a while to figure out that `FunctionName` above needs to refer to the _logical_ name of your Lambda function in the CloudFormation template. Note that the first run will be slow as SAM will fetch the container for your function runtime, subsequent runs should be quicker (adjust `read_timeout`accordingly). 