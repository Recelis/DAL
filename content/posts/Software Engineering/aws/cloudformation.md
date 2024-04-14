+++
title = 'Cloudformation'
date = 2024-03-17T21:38:06+10:00
draft = true
+++

Specify all the infrastructure that you want. The infrastructure is all the resources that you want in a stack. You can define it in a text file and put it into a git file. If something breaks, you can see the last change and debug. 

You can even nest your templates so that different parts of your team work on different sections. 

Creating your website can be automated where you can spin up your entire infrastructure easily. If you want to spin up your website in a different region, you can spin up an instance in a different region.

https://cdkworkshop.com/20-typescript/30-hello-cdk/300-cdk-watch.html

**Creating a cdk Project**

call `cdk init sample-app —typescript`

https://docs.aws.amazon.com/cdk/v2/guide/cli.html#cli-init

**Commands**

`cdk synth`

This command creates the Cloudformation template.

`cdk bootstrap`

This command sets up the S3 bucket where compiled resources are stored.

`cdk deploy —hotswap`

checks if hots3wap deployment can be done instead of a CloudFormation deployment, to save time.

`cdk watch`

Watch is just like calling cdk —hostwap but it watches for any changes in your code.

You’ll have to edit your cdk.json file to allow this if you want your js files to trigger a hotswap.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee966ae5-ba73-4d45-86d9-a25f00e6604b/Untitled.png)

`cdk destroy`

This command will destroy the stack as per each resource’s **Deletion Policy**. CloudWatch logs and the boostrapped stack in the S3 bucket will still be retained.

**Stack Class**

```tsx
export class CdkWorkshopStack extends cdk.Stack {
  constructor(scope: cdk.App, id: string, props?: cdk.StackProps) {
    super(scope, id, props);
    // defines an AWS Lmabda resource
    const hello = new lambda.Function(this, 'HelloHandler', {
      runtime: lambda.Runtime.NODEJS_16_X,
      code: lambda.Code.fromAsset('lambda'),
      handler: 'hello.handler'
    })

    // defines an API Gateway REST API resource backed by our "hello" function.
    new apigw.LambdaRestApi(this, 'Endpoint', {
      handler: hello
    })
  }
}
```

The main  parts are:

- scope
    - this is the scope of the construct. Typically, when you call a resource such as lambda, you’ll pass in `this`
- id
    - This is the unique name of the resource, it has to be unique within the scope.
- props
    - These are any props that you might want to pass in.

**Construct Class**

Define your props and extends Construct.

```tsx
export interface HitCounterProps {
    /** the function for which we want to count url hits */
    downstream: lambda.IFunction
}

export class HitCounter extends Construct {
    /** allows accessing the counter function */
    public readonly handler: lambda.Function

    /** the hit counter table */
    public readonly table: dynamodb.Table

    constructor(scope: Construct, id: string, props: HitCounterProps) {
        super(scope, id)

        const table = new dynamodb.Table(this, 'Hits', {
            partitionKey: {name: 'path', type: dynamodb.AttributeType.STRING},
            removalPolicy: cdk.RemovalPolicy.DESTROY
        })
        this.table = table
        
        this.handler = new lambda.Function(this, 'HitCounterHandler', {
            runtime: lambda.Runtime.NODEJS_14_X,
            handler: 'hitcounter.handler',
            code: lambda.Code.fromAsset('lambda'),
            environment: {
                DOWNSTREAM_FUNCTION_NAME: props.downstream.functionName,
                HITS_TABLE_NAME: table.tableName
            }
        })

        // grant the lambda role read/write permissions to our table
        table.grantReadWriteData(this.handler)

        // grant the lambda role invoke permissions to the downstream function
        props.downstream.grantInvoke(this.handler)
    }
}
```

**Construct Deletion Policy**

Most resources have a removal policy.

removalPolicy: cdk.RemovalPolicy.DESTROY

**Functions**

(From within the Code.fromAsset(’directoryName’)

```jsx
exports.handler = async function(event) {
    console.log("request:", JSON.stringify(event, undefined, 2));
    return {
        statusCode: 200,
        headers: {"Content-Type": "text/plain"},
        body: `Good Night, CDK! You've hit ${event.path}\n`
    }
}
```

**Testing a Construct**

If you want to simply test whether a resource is there: 

You first create your resources and then check whether that resources exists in the template.

```tsx
import * as cdk from 'aws-cdk-lib';
import { Template, Match } from 'aws-cdk-lib/assertions';
import * as lambda from 'aws-cdk-lib/aws-lambda';
import { HitCounter } from '../lib/hitcounter';

test('DynamoDB Table Created', () => {
  const stack = new cdk.Stack();

  new HitCounter(stack, 'MyTestConstruct', {
    downstream: new lambda.Function(stack, 'TestFunction', {
      runtime: lambda.Runtime.NODEJS_14_X,
      handler: 'hello.handler',
      code: lambda.Code.fromAsset('lambda')
    })
  })
  const template = Template.fromStack(stack);
  template.resourceCountIs("AWS::DynamoDB::Table", 1);
})
```

**Assertions**

```tsx
test('Lambda Has Environment Variables', () => {
	// WHEN
  const stack = new cdk.Stack();
  new HitCounter(stack, 'MyTestConstruct', {
    downstream: new lambda.Function(stack, 'TestFunction', {
      runtime: lambda.Runtime.NODEJS_14_X,
      handler: 'hello.handler',
      code: lambda.Code.fromAsset('lambda')
    })
  })
	// THEN
  const template = Template.fromStack(stack);
  const envCapture = new Capture();
  template.hasResourceProperties("AWS::Lambda::Function", {
    Environment: envCapture
  });

  expect(envCapture.asObject()).toEqual(
    {
      Variables: {
        DOWNSTREAM_FUNCTION_NAME: {
          Ref: "TestFunction22AD90FC",
        },
        HITS_TABLE_NAME: {
          Ref: "MyTestConstructHits24A357F0"
        }
      }
    }
  )
})
```