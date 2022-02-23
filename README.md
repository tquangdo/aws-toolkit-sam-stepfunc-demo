# aws-toolkit-sam-stepfunc-demo 🐳

![Stars](https://img.shields.io/github/stars/tquangdo/aws-toolkit-sam-stepfunc-demo?color=f05340)
![Issues](https://img.shields.io/github/issues/tquangdo/aws-toolkit-sam-stepfunc-demo?color=f05340)
![Forks](https://img.shields.io/github/forks/tquangdo/aws-toolkit-sam-stepfunc-demo?color=f05340)
[![Report an issue](https://img.shields.io/badge/Support-Issues-green)](https://github.com/tquangdo/aws-toolkit-sam-stepfunc-demo/issues/new)

## reference
[aws](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/welcome.html)

## toolkit layout
![toolkit](screenshots/toolkit.png)

## SAM app: "lambda-nodejs14.x"
### 1) local (need running docker local app)
#### create
- Ctrl+P -> type `>aws create sam` -> select `create SAM app`
1. select `nodejs14.x`
2. select `x86_64`
3. select `SAM Hello World`
4. select folder `aws-terraform-demo` (example)
5. input app name `lambda-nodejs14.x` (example)
#### run CLI
1. lambda
```shell
lambda-nodejs14.x$ sam local invoke
=>
Mounting /Users/NC00011462/Documents/GitHub/aws-toolkit-sam-demo/lambda-nodejs14.x/hello-world as /var/task:ro,delegated inside runtime container
{"statusCode":200,"body":"{\"message\":\"hello world\"}"}END RequestId: 5f4f75e1-f314-4820-8b10-9f95868ec0d7
REPORT RequestId: 5f4f75e1-f314-4820-8b10-9f95868ec0d7  Init Duration: 0.34 ms  Duration: 175.75 ms     Billed Duration: 176 ms Memory Size: 128 MB     Max Memory Used: 128 MB
```
2. API GW
- `lambda-nodejs14.x$ sam local start-api`
- access `http://127.0.0.1:3000/hello` on browser will show result
```json
{
  message: "hello world"
}
```
#### debug
- **_Way A:_**
- in `lambda-nodejs14.x/hello-world/app.js`: click `AWS: Edit debug configuration > Invoke`
- this screenshot is debugging at breakpoint: `index.js` > line 10 
![sam_debug](screenshots/sam_debug.png)
- **_Way B:_**
- create breakpoint: `lambda-nodejs14.x/hello-world/app.js` > line 32
- vscode left tab > click icon "debug" > click icon "▶️" at top left corner
![sam_debug2](screenshots/sam_debug2.png)
### 2) publish to AWS
#### vscode GUI
- Ctrl+P -> type `>deploy` -> select `AWS: Deploy SAM app`
#### run CLI
```shell
lambda-nodejs14.x$ sam deploy --guided
=>
Stack Name [sam-app]: DTQStackSAMHelloWorld
        AWS Region [us-east-1]: 
        #Shows you resources changes to be deployed and require a 'Y' to initiate deploy
        Confirm changes before deploy [y/N]: y
        #SAM needs permission to be able to create roles to connect to the resources in your template
        Allow SAM CLI IAM role creation [Y/n]: 
        #Preserves the state of previously provisioned resources when an operation fails
        Disable rollback [y/N]: 
        HelloWorldFunction may not have authorization defined, Is this okay? [y/N]: y
        Save arguments to configuration file [Y/n]: 
        SAM configuration file [samconfig.toml]: 
        SAM configuration environment [default]: 
...
Deploy this changeset? [y/N]: y
2022-02-24 00:57:57 - Waiting for stack create/update to complete
CloudFormation events from stack operations
...
Key                 HelloWorldApi                                                                                                          
Description         API Gateway endpoint URL for Prod stage for Hello World function                                                                                                           
Value               https://tbxddu4c9i.execute-api.us-east-1.amazonaws.com/Prod/hello/
```
- access https://tbxddu4c9i.execute-api.us-east-1.amazonaws.com/Prod/hello/ on browser
```json
{
  "message": "hello world"
}
```
#### invoke Lambda
- AWS Explorer > Lambda > select `awstoolkit-HelloWorldFunction...` > Invoke on AWS > terminal tab `OUTPUT` > will see "hello world"

## STEP FUNCTION app: "dtq-nodejs14.x"
### 1) local (need running docker local app)
#### run
```shell
dtq-nodejs14.x$ sam local invoke StockCheckerFunction
=>
Mounting /Users/NC00011462/Documents/GitHub/aws-test-del/dtq-nodejs14.x/functions/stock-checker as /var/task:ro,delegated inside runtime container
{"stock_price":5}END RequestId: a5e923cf-6447-463b-97f1-fd257da8e0dc
REPORT RequestId: a5e923cf-6447-463b-97f1-fd257da8e0dc  Init Duration: 0.26 ms  Duration: 180.12 ms     Billed Duration: 181 ms Memory Size: 128 MB     Max Memory Used: 128 MB
```
#### create step function's json
- Ctrl + P -> type `>step function` -> select `create new step function`
- choose tempate `Hello world`, ..., `Choice state`
- select `json` OR `yaml`
- will ouput *.json OR *.yaml file
- in `dtq-nodejs14.x/statemachine/stock_trader.asl.json`: click `render graph`
![sf_create_sample](screenshots/sf_create_sample.png)
### 2) publish to AWS
- in `dtq-nodejs14.x/statemachine/stock_trader.asl.json`: click `publish to step function`
![sf_publish](screenshots/sf_publish.png)
