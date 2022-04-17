# What are the benefits when I run a Glue job inside VPC?
https://repost.aws/questions/QUNhJvH-SFRwCycHfA2SE0YA#ANEV6WKh4wTyC3POz064QGFA

If you run a job outside of a VPC, the job potentially has direct access to the internet, and a rouge engineer could write code that would write data to some endpoint on the internet that is outside of your organization. There are various ways to address this risk, but one of them is to ensure the job runs on a VPC where you control all data egress.

The other common reason to use a VPC endpoint with your Glue jobs is to enable access to other resources in your VPC (like RDS servers if you need to ingest data from those), or resources on your corporate network (if you have a connection between your VPC and your corporate network).

See the IAM Policies that Control Settings Using Condition Keys in the AWS Glue documentation at the following link. This includes an example of how you can use an IAM policy to ensure that only Glue jobs that have a specific VPC connection are able to be created.

https://docs.aws.amazon.com/glue/latest/dg/using-identity-based-policies.html

All the best with your AWS Glue data engineering!

Tuy nhiên, hãy xem xét thêm: 
https://acloudguru.com/blog/engineering/do-i-really-need-a-vpc
