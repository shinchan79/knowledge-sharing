# Meaning of the red line in RDS DB Connection count metrics 
![](https://live.staticflickr.com/65535/52010226128_4403bd9963_b.jpg)

AWS Support: 

Hello,

Thank you for your continued patience and I really apologies for delay in response!

I understand, you have query related to number of database connection limit and the what does red line indicate. Please correct me if my understanding is wrong.

I would like to inform you that RDS SQl Server follow the maximum connection limit  as per Microsoft which is 32767. Please refer the below document for more information and future reference:

>https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-the-user-connections-server-configuration-option?view=sql-server-ver15

Regarding the red line observed in the cloudwatch metrcis , it is generally ignorable. In general, it is indicator that for this instance class, how many  number of connection are advisable, but this can vary based on customer application\kind of activity they are doing. So, as long you don't see any performance issues in RDS , you can safely ignore this.

I hope this information helps, please do let me know if you have any further question regarding this process.

Awaiting for your kind response!

We value your feedback. Please share your experience by rating this correspondence using the AWS Support Center link at the end of this correspondence. Each correspondence can also be rated by selecting the stars in top right corner of each correspondence within the AWS Support Center.

Best regards,

Pooja S.

Amazon Web Services
