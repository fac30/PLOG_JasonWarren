# CDK Workshop

[Link](https://docs.aws.amazon.com/cdk/v2/guide/hello_world.html)

Step 1: Create your CDK project

In this step, you create a new CDK project. A CDK project should be in its own directory, with its own local module dependencies.

To create a CDK project
From a starting directory of your choice, create and navigate to a directory named hello-cdk:


$ mkdir hello-cdk && cd hello-cdk
Important
Be sure to name your project directory hello-cdk, exactly as shown here. The CDK CLI uses this directory name to name things within your CDK code. If you use a different directory name, you will run into issues during this tutorial.

From the hello-cdk directory, initialize a new CDK project using the CDK CLI cdk init command. Specify the app template and your preferred programming language with the --language option:


TypeScript

JavaScript

Python

Java

C#

Go

$ cdk init app --language typescript
The cdk init command creates a structure of files and folders within the hello-cdk directory to help organize the source code for your CDK app. This structure of files and folders is called your CDK project. Take a moment to explore your CDK project.

If you have Git installed, each project you create using cdk init is also initialized as a Git repository.

During project initialization, the CDK CLI creates a CDK app containing a single CDK stack. The CDK app instance is created using the App construct. The following is a portion of this code from your CDK application file:


TypeScript

JavaScript

Python

Java

C#

Go
Located in bin/hello-cdk.ts:


#!/usr/bin/env node
import 'source-map-support/register';
import * as cdk from 'aws-cdk-lib';
import { HelloCdkStack } from '../lib/hello-cdk-stack';

const app = new cdk.App();
new HelloCdkStack(app, 'HelloCdkStack', {
});
The CDK stack is created using the Stack construct. The following is a portion of this code from your CDK stack file:


TypeScript

JavaScript

Python

Java

C#

Go
Located in lib/hello-cdk-stack.ts:


import * as cdk from 'aws-cdk-lib';
import { Construct } from 'constructs';

export class HelloCdkStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);
    
    // Define your constructs here

  }
}
Step 2: Configure your AWS environment

In this step, you configure the AWS environment for your CDK stack. By doing this, you specify which environment your CDK stack will be deployed to.

First, determine the AWS environment that you want to use. An AWS environment consists of an AWS account and AWS Region.

When you use the AWS CLI to configure security credentials on your local machine, you can then use the AWS CLI to obtain AWS environment information for a specific profile.

To use the AWS CLI to obtain your AWS account ID
Run the following AWS CLI command to get the AWS account ID for your default profile:


$ aws sts get-caller-identity --query "Account" --output text
If you prefer to use a named profile, provide the name of your profile using the --profile option:


$ aws sts get-caller-identity --profile your-profile-name --query "Account" --output text
To use the AWS CLI to obtain your AWS Region
Run the following AWS CLI command to get the Region that you configured for your default profile:


$ aws configure get region
If you prefer to use a named profile, provide the name of your profile using the --profile option:


$ aws configure get region --profile your-profile-name
Next, you will configure the AWS environment for your CDK stack by modifying the HelloCdkStack instance in your application file. For this tutorial, you will hard code your AWS environment information. This is recommended for production environments. For information on other ways to configure environments, see Configure environments to use with the AWS CDK.

To configure the environment for your CDK stack
In your application file, use the env property of the Stack construct to configure your environment. The following is an example:


TypeScript

JavaScript

Python

Java

C#

Go
Located in bin/hello-cdk.ts:


#!/usr/bin/env node
import 'source-map-support/register';
import * as cdk from 'aws-cdk-lib';
import { HelloCdkStack } from '../lib/hello-cdk-stack';

const app = new cdk.App();
new HelloCdkStack(app, 'HelloCdkStack', {
  env: { account: '123456789012', region: 'us-east-1' },
});
Step 3: Bootstrap your AWS environment

In this step, you bootstrap the AWS environment that you configured in the previous step. This prepares your environment for CDK deployments.

To bootstrap your environment, run the following from the root of your CDK project:


$ cdk bootstrap
By bootstrapping from the root of your CDK project, you don't have to provide any additional information. The CDK CLI obtains environment information from your project. When you bootstrap outside of a CDK project, you must provide environment information with the cdk bootstrap command. For more information, see Bootstrap your environment for use with the AWS CDK.

Step 4: Build your CDK app

In most programming environments, you build or compile code after making changes. This isn't necessary with the AWS CDK since the CDK CLI will automatically perform this step. However, you can still build manually when you want to catch syntax and type errors. The following is an example:


TypeScript

JavaScript

Python

Java

C#

Go

$ npm run build
            
> hello-cdk@0.1.0 build
> tsc
Step 5: List the CDK stacks in your app

At this point, you should have a CDK app containing a single CDK stack. To verify, use the CDK CLI cdk list command to display your stacks. The output should display a single stack named HelloCdkStack:


$ cdk list
HelloCdkStack
If you don't see this output, verify that you are in the correct working directory of your project and try again. If you still don't see your stack, repeat Step 1: Create your CDK project and try again.

Step 6: Define your Lambda function

In this step, you import the aws_lambda module from the AWS Construct Library and use the Function L2 construct.

Modify your CDK stack file as follows:


TypeScript

JavaScript

Python

Java

C#

Go
Located in lib/hello-cdk-stack.ts:


import * as cdk from 'aws-cdk-lib';
import { Construct } from 'constructs';
// Import the Lambda module
import * as lambda from 'aws-cdk-lib/aws-lambda';

export class HelloCdkStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // Define the Lambda function resource
    const myFunction = new lambda.Function(this, "HelloWorldFunction", {
      runtime: lambda.Runtime.NODEJS_20_X, // Provide any supported Node.js runtime
      handler: "index.handler",
      code: lambda.Code.fromInline(`
        exports.handler = async function(event) {
          return {
            statusCode: 200,
            body: JSON.stringify('Hello World!'),
          };
        };
      `),
    });
  }
}
Let's take a closer look at the Function construct. Like all constructs, the Function class takes three parameters:

scope – Defines your Stack instance as the parent of the Function construct. All constructs that define AWS resources are created within the scope of a stack. You can define constructs inside of constructs, creating a hierarchy (tree). Here, and in most cases, the scope is this (self in Python).

Id – The construct ID of the Function within your AWS CDK app. This ID, plus a hash based on the function's location within the stack, uniquely identifies the function during deployment. The AWS CDK also references this ID when you update the construct in your app and re-deploy to update the deployed resource. Here, your construct ID is HelloWorldFunction. Functions can also have a name, specified with the functionName property. This is different from the construct ID.

props – A bundle of values that define properties of the function. Here you define the runtime, handler, and code properties.

Props are represented differently in the languages supported by the AWS CDK.

In TypeScript and JavaScript, props is a single argument and you pass in an object containing the desired properties.

In Python, props are passed as keyword arguments.

In Java, a Builder is provided to pass the props. There are two: one for FunctionProps, and a second for Function to let you build the construct and its props object in one step. This code uses the latter.

In C#, you instantiate a FunctionProps object using an object initializer and pass it as the third parameter.

If a construct's props are optional, you can omit the props parameter entirely.

All constructs take these same three arguments, so it's easy to stay oriented as you learn about new ones. And as you might expect, you can subclass any construct to extend it to suit your needs, or if you want to change its defaults.

Step 7: Define your Lambda function URL

In this step, you use the addFunctionUrl helper method of the Function construct to define a Lambda function URL. To output the value of this URL at deployment, you will create an AWS CloudFormation output using the CfnOutput construct.

Add the following to your CDK stack file:


TypeScript

JavaScript

Python

Java

C#

Go
Located in lib/hello-cdk-stack.ts:


// ...

export class HelloCdkStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // Define the Lambda function resource
    // ...

    // Define the Lambda function URL resource
    const myFunctionUrl = myFunction.addFunctionUrl({
      authType: lambda.FunctionUrlAuthType.NONE,
    });

    // Define a CloudFormation output for your URL
    new cdk.CfnOutput(this, "myFunctionUrlOutput", {
      value: myFunctionUrl.url,
    })

  }
}
Warning
To keep this tutorial simple, your Lambda function URL is defined without authentication. When deployed, this creates a publicly accessible endpoint that can be used to invoke your function. When you are done with this tutorial, follow Step 12: Delete your application to delete these resources.

Step 8: Synthesize a CloudFormation template

In this step, you prepare for deployment by synthesizing a CloudFormation template with the CDK CLI cdk synth command. This command performs basic validation of your CDK code, runs your CDK app, and generates a CloudFormation template from your CDK stack.

If your app contains more than one stack, you must specify which stacks to synthesize. Since your app contains a single stack, the CDK CLI automatically detects the stack to synthesize.

If you don't synthesize a template, the CDK CLI will automatically perform this step when you deploy. However, we recommend that you run this step before each deployment to check for synthesis errors.

Before synthesizing a template, you can optionally build your application to catch syntax and type errors. For instructions, see Step 4: Build your CDK app.

To synthesize a CloudFormation template, run the following from the root of your project:


$ cdk synth
Note
If you receive an error like the following, verify that you are in the hello-cdk directory and try again:

--app is required either in command-line, in cdk.json or in ~/.cdk.json
If successful, the CDK CLI will output a YAML–formatted CloudFormation template to stdout and save a JSON–formatted template in the cdk.out directory of your project.

The following is an example output of the CloudFormation template:

AWS CloudFormation template
Note
Every generated template contains an AWS::CDK::Metadata resource by default. The AWS CDK team uses this metadata to gain insight into AWS CDK usage and find ways to improve it. For details, including how to opt out of version reporting, see Version reporting.

By defining a single L2 construct, the AWS CDK creates an extensive CloudFormation template containing your Lambda resources, along with the permissions and glue logic required for your resources to interact within your application.

Step 9: Deploy your CDK stack

In this step, you use the CDK CLI cdk deploy command to deploy your CDK stack. This command retrieves your generated CloudFormation template and deploys it through AWS CloudFormation, which provisions your resources as part of a CloudFormation stack.

From the root of your project, run the following. Confirm changes if prompted:


$ cdk deploy

✨  Synthesis time: 2.69s

HelloCdkStack:  start: Building unique-identifier:current_account-current_region
HelloCdkStack:  success: Built unique-identifier:current_account-current_region
HelloCdkStack:  start: Publishing unique-identifier:current_account-current_region
HelloCdkStack:  success: Published unique-identifier:current_account-current_region
This deployment will make potentially sensitive changes according to your current security approval level (--require-approval broadening).
Please confirm you intend to make the following modifications:

IAM Statement Changes
┌───┬───────────────────────────────────────┬────────┬──────────────────────────┬──────────────────────────────┬───────────┐
│   │ Resource                              │ Effect │ Action                   │ Principal                    │ Condition │
├───┼───────────────────────────────────────┼────────┼──────────────────────────┼──────────────────────────────┼───────────┤
│ + │ ${HelloWorldFunction.Arn}             │ Allow  │ lambda:InvokeFunctionUrl │ *                            │           │
├───┼───────────────────────────────────────┼────────┼──────────────────────────┼──────────────────────────────┼───────────┤
│ + │ ${HelloWorldFunction/ServiceRole.Arn} │ Allow  │ sts:AssumeRole           │ Service:lambda.amazonaws.com │           │
└───┴───────────────────────────────────────┴────────┴──────────────────────────┴──────────────────────────────┴───────────┘
IAM Policy Changes
┌───┬───────────────────────────────────┬────────────────────────────────────────────────────────────────────────────────┐
│   │ Resource                          │ Managed Policy ARN                                                             │
├───┼───────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────┤
│ + │ ${HelloWorldFunction/ServiceRole} │ arn:${AWS::Partition}:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole │
└───┴───────────────────────────────────┴────────────────────────────────────────────────────────────────────────────────┘
(NOTE: There may be security-related changes not in this list. See https://github.com/aws/aws-cdk/issues/1299)

Do you wish to deploy these changes (y/n)? y
Similar to cdk synth, you don't have to specify the AWS CDK stack since the app contains a single stack.

During deployment, the CDK CLI displays progress information as your stack is deployed. When complete, you can go to the AWS CloudFormation console to view your HelloCdkStack stack. You can also go to the Lambda console to view your HelloWorldFunction resource.

When deployment completes, the CDK CLI will output your endpoint URL. Copy this URL for the next step. The following is an example:


...
HelloCdkStack: deploying... [1/1]
HelloCdkStack: creating CloudFormation changeset...

 ✅  HelloCdkStack

✨  Deployment time: 41.65s

Outputs:
HelloCdkStack.myFunctionUrlOutput = https://<api-id>.lambda-url.<Region>.on.aws/
Stack ARN:
arn:aws:cloudformation:Region:account-id:stack/HelloCdkStack/unique-identifier

✨  Total time: 44.34s
Step 10: Interact with your application on AWS

In this step, you interact with your application on AWS by invoking your Lambda function through the function URL. When you access the URL, your Lambda function returns the Hello World! message.

To invoke your function, access the function URL through your browser or from the command line. The following is an example:


$ curl https://<api-id>.lambda-url.<Region>.on.aws/
"Hello World!"%
Step 11: Modify your application

In this step, you modify the message that the Lambda function returns when invoked. You perform a diff using the CDK CLI cdk diff command to preview your changes and deploy to update your application. You then interact with your application on AWS to see your new message.

Modify the myFunction instance in your CDK stack file as follows:


TypeScript

JavaScript

Python

Java

C#

Go
Located in lib/hello-cdk-stack.ts:


// ...

export class HelloCdkStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // Modify the Lambda function resource
    const myFunction = new lambda.Function(this, "HelloWorldFunction", {
      runtime: lambda.Runtime.NODEJS_20_X, // Provide any supported Node.js runtime
      handler: "index.handler",
      code: lambda.Code.fromInline(`
        exports.handler = async function(event) {
          return {
            statusCode: 200,
            body: JSON.stringify('Hello CDK!'),
          };
        };
      `),
    });

    // ...
Currently, your code changes have not made any direct updates to your deployed Lambda resource. Your code defines the desired state of your resource. To modify your deployed resource, you will use the CDK CLI to synthesize the desired state into a new AWS CloudFormation template. Then, you will deploy your new CloudFormation template as a change set. Change sets make only the necessary changes to reach your new desired state.

To preview your changes, run the cdk diff command. The following is an example:


$ cdk diff
Stack HelloCdkStack
Hold on while we create a read-only change set to get a diff with accurate replacement information (use --no-change-set to use a less accurate but faster template-only diff)
Resources
[~] AWS::Lambda::Function HelloWorldFunction HelloWorldFunctionunique-identifier
 └─ [~] Code
     └─ [~] .ZipFile:
         ├─ [-] 
                exports.handler = async function(event) {
                    return {
                      statusCode: 200,
                      body: JSON.stringify('Hello World!'),
                    };
                };
                
         └─ [+] 
                exports.handler = async function(event) {
                    return {
                      statusCode: 200,
                      body: JSON.stringify('Hello CDK!'),
                    };
                };
                

✨  Number of stacks with differences: 1
To create this diff, the CDK CLI queries your AWS account account for the latest AWS CloudFormation template for the HelloCdkStack stack. Then, it compares the latest template with the template it just synthesized from your app.

To implement your changes, run the cdk deploy command. The following is an example:


$ cdk deploy

✨  Synthesis time: 2.12s

HelloCdkStack:  start: Building unique-identifier:current_account-current_region
HelloCdkStack:  success: Built unique-identifier:current_account-current_region
HelloCdkStack:  start: Publishing unique-identifier:current_account-current_region
HelloCdkStack:  success: Published unique-identifier:current_account-current_region
HelloCdkStack: deploying... [1/1]
HelloCdkStack: creating CloudFormation changeset...

 ✅  HelloCdkStack

✨  Deployment time: 26.96s

Outputs:
HelloCdkStack.myFunctionUrlOutput = https://unique-identifier.lambda-url.<Region>.on.aws/
Stack ARN:
arn:aws:cloudformation:Region:account-id:stack/HelloCdkStack/unique-identifier

✨  Total time: 29.07s
To interact with your application, repeat Step 10: Interact with your application on AWS. The following is an example:


$ curl https://<api-id>.lambda-url.<Region>.on.aws/
"Hello CDK!"%
Step 12: Delete your application

In this step, you use the CDK CLI cdk destroy command to delete your application. This command deletes the CloudFormation stack associated with your CDK stack, which includes the resources you created.

To delete your application, run the cdk destroy command and confirm your request to delete the application. The following is an example:


$ cdk destroy
Are you sure you want to delete: HelloCdkStack (y/n)? y
HelloCdkStack: destroying... [1/1]

 ✅  HelloCdkStack: destroyed
