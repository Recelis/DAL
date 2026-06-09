# Lambda

((I reference a chat with Claude)

## Dependencies

AWS Lambda is one of the main building blocks of AWS. It basically is a bit of code run that has clear defined inputs and outputs, and is run in its own container for max of 15 minutes.

The bit of code is called a `handler`.

## Implementations

For TypeScript and a Lambda off an `SQS queue`. It would look like this:

```typescript
import { SQSHandler, SQSBatchResponse } from "aws-lambda";

export const handler: SQSHandler = async (event): Promise<SQSBatchResponse> => {
  const failures: string[] = [];

  for (const record of event.Records) {
    try {
      const message = JSON.parse(record.body) as MyMessageType;
      await processMessage(message);
    } catch (err) {
      // Return failed message IDs instead of throwing — avoids
      // failing the whole batch
      failures.push(record.messageId);
    }
  }

  return {
    batchItemFailures: failures.map((id) => ({ itemIdentifier: id })),
  };
};
```

This can be fed back to a `Dead Letter Queue`.

Other common patterns are triggering it from API Gateway:

```typescript
import { APIGatewayProxyHandlerV2, APIGatewayProxyResultV2 } from "aws-lambda";

export const handler: APIGatewayProxyHandlerV2 = async (event) => {
  const userId = event.pathParameters?.id;
  const filter = event.queryStringParameters?.filter;
  const body = JSON.parse(event.body ?? "{}") as { name: string };

  return {
    statusCode: 200,
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ userId, filter, name: body.name }),
  };
};
```

Or S3:

```typescript
import { S3Handler } from "aws-lambda";
import { S3Client, GetObjectCommand } from "@aws-sdk/client-s3";

const s3 = new S3Client({});

export const handler: S3Handler = async (event) => {
  for (const record of event.Records) {
    const bucket = record.s3.bucket.name;
    const key = decodeURIComponent(record.s3.object.key.replace(/\+/g, " "));

    const obj = await s3.send(
      new GetObjectCommand({ Bucket: bucket, Key: key }),
    );
    // process obj...
  }
};
```

Or EventBridge:

```typescript
import { EventBridgeHandler } from "aws-lambda";

interface OrderDetail {
  orderId: string;
  amount: number;
}

export const handler: EventBridgeHandler
  "OrderPlaced",       // detail-type
  OrderDetail,         // detail shape
  void
> = async (event) => {
  const { orderId, amount } = event.detail; // fully typed
  await processOrder(orderId, amount);
};
```

In CDK, we define our stack with a lambda in several ways:

`NodejsFunction` - where esbuild bundles your code for you:

```typescript
const fn = new NodejsFunction(this, "MyFunction", {

      // ── NodejsFunction-specific ──────────────────────────────────────

      // Path to the entry .ts/.js file (auto-detected if omitted and
      // your file sits next to the stack file)
      entry: path.join(__dirname, "../src/handlers/my-handler.ts"),

      // Name of the exported handler function (default: "handler")
      handler: "handler",

      // esbuild bundling options
      bundling: {
        minify: true,
        sourceMap: true,
        target: "node20",
        // Packages NOT to bundle (rely on Lambda runtime or a layer instead)
        externalModules: ["@aws-sdk/*"],
        // Packages to bundle even if marked external
        nodeModules: ["zod"],
        // Pass env vars into the bundle at build time
        define: { "process.env.STAGE": JSON.stringify("prod") },
        // ESM output (requires runtime NODEJS_18_X+)
        format: OutputFormat.ESM,
        // Banner injected at top of output (useful for __dirname shim in ESM)
        banner: "import { createRequire } from 'module'; const require = createRequire(import.meta.url);",
      },

      // ── Runtime / compute ────────────────────────────────────────────

      runtime: lambda.Runtime.NODEJS_22_X,  // default for NodejsFunction
      architecture: lambda.Architecture.ARM_64,  // cheaper than X86_64
      memorySize: 512,        // MB — default 128, max 10240
      ephemeralStorageSize: cdk.Size.mebibytes(1024), // /tmp size, default 512

      // ── Timeouts ─────────────────────────────────────────────────────

      timeout: cdk.Duration.seconds(30),  // default 3s, max 900s

      // ── Identity / permissions ────────────────────────────────────────

      // Provide your own role (CDK creates one by default)
      // role: myRole,

      // Inline policies added to the auto-created role
      initialPolicy: [
        new iam.PolicyStatement({
          actions: ["s3:GetObject"],
          resources: ["arn:aws:s3:::my-bucket/*"],
        }),
      ],

      // ── Environment variables ─────────────────────────────────────────

      environment: {
        TABLE_NAME: "my-table",
        LOG_LEVEL: "info",
      },

      // KMS key to encrypt environment variables
      // environmentEncryption: myKey,

      // ── Networking ────────────────────────────────────────────────────

      // Place the Lambda inside a VPC
      // vpc: myVpc,
      // vpcSubnets: { subnetType: ec2.SubnetType.PRIVATE_WITH_EGRESS },
      // securityGroups: [mySecurityGroup],

      // Allow all outbound traffic (default: true)
      allowAllOutbound: true,

      // ── Concurrency ───────────────────────────────────────────────────

      // Hard cap on concurrent executions (0 = throttle all)
      reservedConcurrentExecutions: 10,

      // ── Error handling / retries ──────────────────────────────────────

      // For async invocations
      retryAttempts: 2,                          // 0–2
      maxEventAge: cdk.Duration.hours(6),        // max time event sits in queue

      // Dead letter target for failed async invocations
      deadLetterQueueEnabled: true,
      // deadLetterQueue: myQueue,   // provide your own SQS queue
      // deadLetterTopic: myTopic,   // or an SNS topic

      // ── Logging ───────────────────────────────────────────────────────

      loggingFormat: lambda.LoggingFormat.JSON,  // structured logs
      // applicationLogLevelV2: lambda.ApplicationLogLevel.INFO,
      // systemLogLevelV2: lambda.SystemLogLevel.WARN,

      // Bring your own log group (lets you set retention)
      logGroup: new logs.LogGroup(this, "FnLogGroup", {
        retention: logs.RetentionDays.ONE_WEEK,
        removalPolicy: cdk.RemovalPolicy.DESTROY,
      }),

      // ── Layers ────────────────────────────────────────────────────────

      layers: [
        // lambda.LayerVersion.fromLayerVersionArn(this, "SharedLayer", "arn:...")
      ],

      // ── Event sources ─────────────────────────────────────────────────

      // Attach event sources directly on the function
      events: [
        // new SqsEventSource(myQueue),
        // new S3EventSource(myBucket, { events: [s3.EventType.OBJECT_CREATED] }),
      ],

      // ── Naming / metadata ─────────────────────────────────────────────

      functionName: "my-service-my-function",  // omit to let CDK generate
      description: "Handles user creation requests",

      // ── Snapstart (Java only) / code signing ──────────────────────────
      // codeSigningConfig: mySigningConfig,

      // ── Tracing ───────────────────────────────────────────────────────

      tracing: lambda.Tracing.ACTIVE,  // X-Ray tracing

      // ── Filesystem (EFS) ──────────────────────────────────────────────

      // filesystem: lambda.FileSystem.fromEfsAccessPoint(accessPoint, "/mnt/data"),
    });

    // Grant permissions after the fact (alternative to initialPolicy)
    // myTable.grantReadWriteData(fn);
    // myBucket.grantRead(fn);
  }
}
```

Typically, you'd grant permissions for your Lambda using a `grant*` method:

```typescript
myTable.grantReadWriteData(fn);
myBucket.grantRead(fn);
myQueue.grantSendMessages(fn);
mySecret.grantRead(fn);
```

Other ways are `Code.fromAsset` where you have to build your own code and CDK manages the upload of code to the S3 bucket entirely.

```typescript
new lambda.Function(this, "MyFunction", {
  runtime: lambda.Runtime.NODEJS_22_X,
  handler: "index.handler",
  code: lambda.Code.fromAsset(path.join(__dirname, "../dist")), // pre-built JS
});
```

`Code.fromBucket` where you've uploaded your code to a bucket first and the CDK just references the bucket location.

```typescript
new lambda.Function(this, "MyFunction", {
  runtime: lambda.Runtime.NODEJS_22_X,
  handler: "index.handler",
  code: lambda.Code.fromBucket(bucket, "my-function.zip"),
});
```

There are others like `fromInline` and `DockerImageFunction`.

Or you can use `Higher Order Constructs (L3)`:

```typescript
// wraps lambda with apigateway
import { LambdaRestApi } from "aws-cdk-lib/aws-apigateway";
```

## Dependents

A common way to ensure typing in Lambda is with `zod`:

```typescript
import { z } from "zod";

const CreateUserSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
});

type CreateUserRequest = z.infer<typeof CreateUserSchema>; // type derived from schema

export const handler: APIGatewayProxyHandlerV2 = async (event) => {
  const parsed = CreateUserSchema.safeParse(JSON.parse(event.body ?? "{}"));

  if (!parsed.success) {
    return {
      statusCode: 400,
      body: JSON.stringify({ error: parsed.error.flatten() }),
    };
  }

  const { name, email } = parsed.data; // fully typed and validated
  await createUser({ name, email });

  return { statusCode: 201, body: JSON.stringify({ ok: true }) };
};
```

With DynamoDB streams:

```typescript
import { z } from "zod";

const UserSchema = z.object({
  userId: z.string(),
  age: z.number(),
  active: z.boolean(),
});

const raw = unmarshall(
  record.dynamodb!.NewImage! as Record<string, AttributeValue>,
);
const result = UserSchema.safeParse(raw);

if (!result.success) {
  console.error("Unexpected shape from DynamoDB", result.error);
  continue;
}

const user = result.data; // typed as User
```

## Interactions

You can view a Lambda within the AWS Console, see where it is triggered from, what its config is. You can even pass variables inside.
