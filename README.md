
# lambda-s3-trigger-sqs-destination-demo

S3 as the Lambda trigger, SQS as the Lambda destination

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/s3-4.png)

###  Create role

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/s3-2.png)

###  Create S3 Bucket

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/s3-1.png)

###  Add S3 as the trigger

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/s3-3.png)

###  Create SQS

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/sqs-1.png)


###  Add SQS as the destination

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/sqs-2.png)

###  Codes sample (node.js)

```
exports.handler = function(event, context, callback) {
   console.log("Incoming Event: ", event);
   const bucket = event.Records[0].s3.bucket.name;
   const filename = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, ' '));
   const message = `Test file is uploaded in - ${bucket} -> ${filename}`;
   console.log(message);

    if (event.Success) {
        console.log("Success");
        context.callbackWaitsForEmptyEventLoop = false;
        callback(null, message);
    } else {
        console.log("Failure");
        context.callbackWaitsForEmptyEventLoop = false;
        callback(new Error("Failure from event, Success = false, I am failing!"), 'Destination Function Error Thrown');
    }
    
};
```

###  Test

1. Upload 3 Png files to S3 bucket

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/test-1.png)

2. CloudWatch

```
2021-08-02T14:21:58.631Z	1e8ba8e6-b6d9-48bc-ad8c-1cb603169e5b	INFO	File is uploaded in - test-lambda-s3-2 -> s3-1.png

2021-08-02T14:22:15.283Z	883fe5e7-c11f-4daa-9dac-40a0f2510a5e	INFO	File is uploaded in - test-lambda-s3-2 -> s3-2.png

2021-08-02T14:22:29.795Z	a4554ef9-480c-41f3-9897-07282c7f5d55	INFO	File is uploaded in - test-lambda-s3-2 -> s3-3.png

```

3. SQS

![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/test-2.png)


![enter image description here](https://github.com/vikki-xiaohua/AWS-series-s3-lambda-trigger-destination-demo/blob/main/image/test-3.png)

### Reference

https://aws.amazon.com/blogs/compute/introducing-aws-lambda-destinations/


## Also see [Api-Gateway-Dynamodb-Lambda](https://github.com/vikki-xiaohua/AWS-series-api-gateway-dynamodb-lambda-demo)


## Todo

Refine the codes



