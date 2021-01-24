# AUTOMATE AWS LAMBDA DEPLOYMENT USING GITHUB ACTION AND SERVERLESS FRAMEWORK

Continuous integration (CI) and continuous delivery (CD) embody a culture, set of operating principles, and collection of practices that enable application development teams to deliver code changes more frequently and reliably. The implementation is also known as the CI/CD pipeline. 

### AWS LAMBDA
AWS Lambda is one of the numerous services offered by Amazon Web Services(AWS), an on-demand cloud computing platform. AWS Lambda lets you upload your code and it takes care of everything required to run and scale your code with high availability without you having to provision or manage servers and you only pay for the compute time you consume.

### Github Action
GitHub Actions makes it easy to automate all your software workflows, now with world-class CI/CD. Build, test, and deploy your code right from GitHub. Make code reviews, branch management, and issue triaging work the way you want.

### Serverless
The Serverless Framework is a free and open-source web framework written using Node.js. Serverless is the first framework developed for building applications on AWS Lambda, a serverless computing platform provided by Amazon as a part of Amazon Web Services.

## Quick Start

1. Install serverless globally

```bash
sudo npm install -g serverless
```
2. Create a AWS lambda Template
```bash
serverless create --template aws-nodejs 
```
3. Update your serverless.yaml
```bash
service: aws-lamda-deployment

provider:
  name: aws
  runtime: nodejs12.x
  lambdaHashingVersion: 20201221
  stage: dev
  region: us-west-2

functions:
  hello:
    handler: src/handler.hello
    timeout: 60
    memorySize: 128
    environment:
     APIG_DEPLOYMENT_ID: 123456344
```
4. Create folder called .github/workflows and create file called deploy.yml
```bash
name: Deploy Lambda

on:
  push:
    branches:
      - main

jobs:
  deploy:
    name: deploy
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x]
    steps:

    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}

    - name: serverless deploy
      uses: serverless/github-action@master
      with:
        args: deploy
      env:
        # SERVERLESS_ACCESS_KEY: ${{ secrets.SERVERLESS_ACCESS_KEY }}
        # or if using AWS credentials directly
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```
5. Now update aws_access_key and aws_secrets_key on github.
6. commit your code and push to github, github action deploy your code to AWS.

## References
https://medium.com/better-programming/set-up-a-ci-cd-pipeline-for-aws-lambda-with-github-actions-and-serverless-in-under-5-minutes-fd070da9d143


