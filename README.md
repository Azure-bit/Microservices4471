# StockTracker

This project contains the source code and supporting files for a serverless stock tracking application deployed using the Serverless Application Model (SAM) CLI. The application includes Lambda functions, API Gateway, and other AWS resources as defined in the template.yaml file.

## Project Structure

- **frontend**: Contains a file `testacc3.py` that checks all the services registered in the associated AWS CloudMap namespace and invokes them locally. This is one of the most important tests in the application.
- **compare_stocks**: Contains the source code for the Lambda function that compares stocks.
- **discover_services**: Contains the source code for the Lambda function that discovers available services.
- **create_stock**: Contains the source code for the Lambda function that creates a new stock entry.
- **delete_stock**: Contains the source code for the Lambda function that deletes a stock entry.
- **get_stock**: Contains the source code for the Lambda function that retrieves a single stock's details.
- **get_stocks**: Contains the source code for the Lambda function that retrieves all stock details.
- **update_stock**: Contains the source code for the Lambda function that updates an existing stock.
- **events**: Sample invocation events for testing the Lambda functions.
- **tests**: Contains unit and integration tests for the application code.
- **template.yaml**: The SAM template that defines the application's AWS resources, such as Lambda functions, API Gateway, and AWS CloudMap services.

All Lambda functions and resources are defined in the template.yaml file, and you can add or modify resources through the same template.

## Deploy the Application

The Serverless Application Model Command Line Interface (SAM CLI) is used to build and deploy this application. SAM simplifies the management of Lambda applications by emulating Lambda's environment and managing the associated AWS infrastructure.

### Prerequisites

Before deploying, ensure that the following tools are installed:

- **SAM CLI**
- **Python 3**
- **Docker**

### Deploy for the First Time

1. Build the application with the following command:

    ```bash
    sam build --use-container
    ```

2. Deploy the application to AWS:

    ```bash
    sam deploy --guided
    ```

During the deployment, you will be prompted to provide several configuration details:

- **Stack Name**: Choose a unique name for the CloudFormation stack.
- **AWS Region**: Specify the AWS region to deploy to.
- **Confirm changes before deploy**: Choose whether to manually approve changes before deployment.
- **Allow SAM CLI IAM role creation**: SAM will create IAM roles required for Lambda to access necessary AWS services.
- **Save arguments to samconfig.toml**: Save configuration for future deployments.

Once the deployment is complete, you will be provided with the API Gateway Endpoint URL.

## Run and Test Locally

### Build the Application

To build your application locally, run the following command:

```bash
    sam build --use-container
```

This installs dependencies from the create_stock/requirements.txt, update_stock/requirements.txt, and other Lambda-specific files, creating a deployment package in the .aws-sam/build directory.
Test Functions Locally

You can invoke functions directly with the following command (provide a test event):

```bash
sam local invoke FunctionName --event events/event.json
```

### Emulate the API Gateway Locally

To simulate the API Gateway locally, start the API server on port 3000:

```bash
sam local start-api
```

You can then use curl or any other HTTP client to test your endpoints:

```bash
curl http://localhost:3000/your-path
```

The SAM CLI reads the template.yaml file to define API routes and their corresponding Lambda functions.

## Running the Important Test: testacc3.py

The application includes an important test located in frontend/testacc3.py. This test checks all the services registered in the associated AWS CloudMap namespace and invokes them locally. This is a critical test to ensure the correctness and availability of the services in the system.

To run this test, ensure that all services are correctly registered in the AWS CloudMap namespace and invoke the test:

```bash
python frontend/testacc3.py
```

## Add More Resources

The template.yaml file defines the AWS resources used by the application. You can modify this template to add additional resources (such as more Lambda functions, databases, etc.) or use standard AWS CloudFormation resources if needed.
Fetching Logs

To retrieve logs generated by your Lambda functions, use the sam logs command:

```bash
sam logs -n FunctionName --stack-name "stocktracker" --tail
```

This will stream the logs to your terminal for easier debugging (note that the stack name needs to match the name you set during deployment).

## Running Tests

The tests folder contains unit and integration tests.

### Unit Tests

To run unit tests, install the required dependencies and run the following:

```bash
pip install -r tests/requirements.txt --user
python -m pytest tests/unit -v
```

### Integration Tests

Integration tests require the application to be deployed. Set the AWS_SAM_STACK_NAME environment variable to the stack name and then run the integration tests:

```bash
AWS_SAM_STACK_NAME="stocktracker" python -m pytest tests/integration -v
```

## Clean Up

To delete the application and its AWS resources, use the sam delete command:

```bash
sam delete --stack-name "stocktracker"
```

Note that the name of the stack should match the one you set at deployment.