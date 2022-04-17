# I can not insert value into my table ("default"."aaaa")
## Question: 
SYNTAX_ERROR: line 1:80: Column 'aa' cannot be resolved. If a data manifest file was generated at 's3://csv-parge/parquet/athena/Unsaved/2022/04/11/f6b9b28c-014b-4a7e-af4e-9be6d1c7c114-manifest.csv', you may need to manually clean the data from locations specified in the manifest. Athena will not delete data in your account.
This query ran against the "default" database, unless qualified by the query. Please post the error message on our forum
or contact customer support
with Query Id:
## Answer:

(From AWS Support)
Hello,

Thank you for reaching out AWS Premium Support, my name is Mukesh and I will be assisting you with your case.

From the case correspondence, I understand that you are getting an error column cannot be resolved while inserting the values into the table.

Upon checking the query details by using our internal tools, I can see that you have enclosed values to match in double quotes. I would like to inform you that in Athena for DML queries double quotes (") are used as quote identifier for columns, therefore values are being treated as columns and thus the error.

insert into aaaa ("dispatching_base_num", "pickup_date", "locationid") values ("aa", "vvv", 123) 

Kindly change it to

INSERT INTO aaaa ("dispatching_base_num", "pickup_date", "locationid") VALUES (‘aa’, ‘vvv’, 123)

To identify a string I would request you try using single quote (') instead of double quotes (").

I hope you find the above information helpful. However, if you have any further queries or issues please update me on the case. I will be happy to help.

Thank you and have a nice day!

We value your feedback. Please share your experience by rating this correspondence using the AWS Support Center link at the end of this correspondence. Each correspondence can also be rated by selecting the stars in top right corner of each correspondence within the AWS Support Center.

Best regards,

Mukesh N.

Amazon Web Services
