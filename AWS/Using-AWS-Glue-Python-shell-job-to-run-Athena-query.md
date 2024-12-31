1. Set up connection: Reference: https://dev.classmethod.jp/articles/run-aws-glue-job-inside-vpc/ (you have to setup S3 and Athena VPC endpoint (note: can use Glue endpoint instead of Athena endpoint).

2. Create the job:
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e634f70d-66b4-4f66-9f59-7a54d3d1bb40/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220417%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220417T093546Z&X-Amz-Expires=86400&X-Amz-Signature=5b1cfba6ce46f5dedd732bf6b551d080882319e27f73f7f16d7d2b5b0f5fc900&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- Choose Python Shell script editor
- Choose Create
- Click Job detail:
- Enter the Name
- Choose IAM Role
- Expand the Advanced properties
- Choose the connection created in Connections part
- Choose tab Script
- Paste the sript into the editor
- Save
- Choose Run to run the job 

Script demo:
```
import time
import boto3

query = 'SELECT * FROM cloudtrail_logs_aws_cloudtrail_logs_741023408660_05daf6c9 LIMIT 5'
DATABASE = 'default'
output='s3://yen-bucket-demo/'

query = "SELECT * FROM default.tb"
client = boto3.client('athena')
# Execution
response = client.start_query_execution(
    QueryString=query,
    QueryExecutionContext={
        'Database': DATABASE
    },
    ResultConfiguration={
        'OutputLocation': output,
    }
)
print(response)
``` 

Note: the SG rule for Glue to connect to S3: Must have at least 1 inbound that allow all TCP! 
