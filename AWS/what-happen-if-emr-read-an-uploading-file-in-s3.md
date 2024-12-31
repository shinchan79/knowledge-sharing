# What happen if EMR read an uploading/is being replaced file in S3

Dear AWS,

When study the documentations, I know that now I do not have to use the EMRFS Consistent view because S3 support read-after-write consistency. However, I want to ask a question:

what will happen if reading occurs during writing with the patterns (1) to (3) below?

( 1) When referencing when creating new data to newly created S3
(2) When referencing when updating
　existing S3 data
(3) When referencing when deleting deleted files.

For example, what will happen if EMR read an uploading file from S3? Is there any mechanism in EMR that wait for uploading process complete?

## Answer from AWS

Dear Customer,

Greetings for the day!

Please accept my sincere apologies for the extreme delay in response, due to large influx of cases, the delay crept in.

Please find the below response to all your queries, all these have been tested on running EMR cluster before being provided. The reference documentation considered here have been added in the footnotes.

For my test, I used a 300 MB text file.

( 1) When referencing when creating new data to newly created S3 –

> For this operation, I tried to list a file while it was being uploaded, however as soon as the upload was complete, I was able to list the object. Hence, the read after write consistency applies here. The same has been quoted in the document mentioned [1]

“A process writes a new object to Amazon S3 and immediately lists keys within its bucket. The new object appears in the list.”

(2) When referencing when updating
　existing S3 data

> I tried to upload a copy of the file, and then made some changes locally, and started a new upload, while the upload was happening, I downloaded a present copy available in S-3, and received the old data. When the upload was complete, I tried again downloading a copy, and this time I received the new data. Hence, here as well the read after write consistency was ensured.

Quoting from the document[1] as well for evidence, “Updates to a single key are atomic. For example, if you make a PUT request to an existing key from one thread and perform a GET request on the same key from a second thread concurrently, you will get either the old data or the new data, but never partial or corrupt data.”

(3) When referencing when deleting deleted files.

> I tried deleting a file, and immediately tried to read it, and I got a 404 Key not found- “fatal error: An error occurred (404) when calling the HeadObject operation: Key “application_1639695381008_42415_1.inprogress” does not exist”. Same as been quoted in the page link provided:

Quoting from document [1], “A process deletes an existing object and immediately tries to read it. Amazon S3 does not return any data because the object has been deleted. “

(4) For example, what will happen if EMR read an uploading file from S3? Is there any mechanism in EMR that wait for uploading process complete?

> For the same point which we saw for 1st, the file is not available until fully written. For this I will suggest adding retries in your code, Like I used below to add in my aws config to enable retries.

[myConfigProfile]
region = us-east-1
max_attempts = 10
retry_mode = standard

You can add this in the default config, and that will add retry to your commands. You can even do this through boto config, if you are using the boto library [3].

I hope, I was able to answer all your questions. P. I will request you to test these out, and see if my observations match with you, or you see any differences, and I will be glad to validate them. Thank you so much, and I apologize again for the delayed response.

[1] https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html#ConsistencyModel

[2] https://aws.amazon.com/blogs/aws/amazon-s3-update-strong-read-after-write-consistency/

[3] https://boto3.amazonaws.com/v1/documentation/api/latest/guide/retries.html

We value your feedback. Please share your experience by rating this correspondence using the AWS Support Center link at the end of this correspondence. Each correspondence can also be rated by selecting the stars in top right corner of each correspondence within the AWS Support Center.

Best regards,

Anirudh C.

Amazon Web Services
