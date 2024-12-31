As of October 6, 2022, AWS does not yet support the function of changing admin user email on the AWS QuickSight console.
Therefore, to change the admin user’s email, you use the AWS CLI command [aws quicksight update-user] as shown in the following sample:

```
aws quicksight update-user --user-name <admin_user_name> --aws-account-id <account_id> --email <the_desired_email> --namespace default --role ADMIN
```

References:

　[update-user]
　https://awscli.amazonaws.com/v2/documentation/api/latest/reference/quicksight/update-user.html
