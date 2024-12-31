# I can not stop EC2 by using EventBridge

I am setting rule1 (us-east-1) to run AWS-StopEC2Instance run book. However, when I specify account ID in ARN resources of the role policy, I get access denied. However, when I use alias *, it run OK.

Am I right if I think that only custom runbook can I specify account ID in the resource ARN, and if I want to use AWS defined runbook, I have to replace account ID to “*”? Can you specify for me the document that talk about this behavior?

Here is my CloudTrail log:

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "xxxxxxxxx",
        "arn": "arn:aws:sts::<account_id>:assumed-role/EventBridge-ExecutionRunbook-ForDemo/088c41b2a8e4309993e75be23xxxxx",
        "accountId": "<account_id>",
        "accessKeyId": "xxxxxxxxx",
        "sessionContext": {
            "sessionIssuer": {
                "type": "Role",
                "principalId": "xxxxxxxxx",
                "arn": "arn:aws:iam::<account_id>:role/EventBridge-ExecutionRunbook-ForDemo",
                "accountId": "<account_id>",
                "userName": "EventBridge-ExecutionRunbook-ForDemo"
            },
            "webIdFederationData": {},
            "attributes": {
                "creationDate": "2022-05-24T06:10:53Z",
                "mfaAuthenticated": "false"
            }
        },
        "invokedBy": "events.amazonaws.com"
    },
    "eventTime": "2022-05-24T06:10:53Z",
    "eventSource": "ssm.amazonaws.com",
    "eventName": "StartAutomationExecution",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "events.amazonaws.com",
    "userAgent": "events.amazonaws.com",
    "errorCode": "AccessDenied",
    "errorMessage": "User: arn:aws:sts::<account_id>:assumed-role/EventBridge-ExecutionRunbook-ForDemo/088c41b2a8e4309993e75be23dxxxxx is not authorized to perform: ssm:StartAutomationExecution on resource: arn:aws:ssm:us-east-1::automation-definition/AWS-StopEC2Instance:$DEFAULT because no identity-based policy allows the ssm:StartAutomationExecution action",
    "requestParameters": null,
    "responseElements": null,
    "requestID": "79230b28-d7e8-4f3d-a8aa-d3f5exxx,
    "eventID": "6660e213-2b88-42c0-847f-7f0b8e9xxx",
    "readOnly": false,
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "<account_id>",
    "eventCategory": "Management"
}
```

Thank you so much!

## Answer from AWS: 
Hi Team,

Greetings!

Thank you for contacting Amazon Web Services Premium Support. My name is Rahul, and I’m here today to assist you with this case.
Reading through the case description, I understand that you are unable to execute AWS Systems Manager Runbook ‘AWS-StopEC2Instance’ when specifying your account ID as part of the ARN.

Please let me know if I misunderstood anything and this needs any revision.

-You have an Event Rule which targets SSM automation: AWS-StopEC2Instance:$DEFAULT. I checked the CloudTrail history for ‘StartAutomationExecution’ API[1] and can see the following error:

```
"errorCode": "AccessDenied",
    "errorMessage": "User: arn:aws:sts::<account_id>:assumed-role/EventBridge-ExecutionRunbook-ForDemo/088c41b2a8e4309993e75be23dxxxxx is not authorized to perform: ssm:StartAutomationExecution on resource: arn:aws:ssm:us-east-1::automation-definition/AWS-StopEC2Instance:$DEFAULT because no identity-based policy allows the ssm:StartAutomationExecution action"
```

-I did some research on this internally and can confirm that- “The ARN format of AWS-published Systems Manager Automation documents does not support specifying your account ID as part of the ARN.”

The ARN format of Systems Manager Automation documents not published by AWS will work with the account ID (irrespective of whether you are the owner, or you are using Systems Manager documents shared by other accounts). Hence your understanding is correct about specifying AWS account id in the ARN of the automation document.

IAM Policy Example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "ssm:StartAutomationExecution",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:us-east-1:*:automation-definition/AWS-StopEC2Instance:$DEFAULT"
            ]
}
```

Unfortunately, I was unable to locate any public documentation to support this assertion. This email can be used as a reference for internal purposes. If you still want a public AWS document, please let me know and I will request one from our internal team.

I hope the information provided is beneficial. Furthermore, if I have not addressed any other problems or questions you may have regarding this case, please update the case communication with your use-case, and I would be happy to work on it.

Have a wonderful and safe day ahead!

We value your feedback. Please share your experience by rating this correspondence using the AWS Support Center link at the end of this correspondence. Each correspondence can also be rated by selecting the stars in top right corner of each correspondence within the AWS Support Center.

Best regards,
Rahul C.
Amazon Web Services
