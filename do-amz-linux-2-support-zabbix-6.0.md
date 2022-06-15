# Do AMZ Linux 2 support Zabbix 6.0?
## Question:

I am planning to use Zabbix 6.0 on AMZ Linux 2. However, I found this docs:

https://www.zabbix.com/documentation/current/en/manual/installation/install_from_packages/rhel_centos

—–

Verify CA encryption mode doesn’t work on RHEL 7 with MySQL due to older MySQL libraries.

—–

I am searching about the information that on which distro AMZ Linux 2 based on. However, I only found this article (not AWS official docs): https://linuxhint.com/what_is_amazon_linux_2/
This docs said that AMZ Linux based on RHEL 7 => Zabbix 6.0 may not work on this distro.

For confirmation purpose, I want to ask:

    on which distro & which version of the distro that AMZ Linux 2 based on?
    Do you have any information about the compatibility of AMZ Linux 2 and Zabbix 6.0?
    
 ## Answer from AWS:
 
Hello,

Hope this email finds you well !!

Warm greetings from AWS Premium Support. My name is Vivek and I’ll be assisting you on this case today.

From the case description, I do understand that you are currently looking out for details and compatibility of Zabbix on Amazon Linux 2. Please correct me if I have misunderstood anything.

To begin with, I can say that it almost closely based out of RHEL and certainly due to data integrity we are not allowed to share these details.

As you might be aware of that Zabbix is a third party software and all AWS engineers might not have the expertise on the same. Hence assistance on such tools and services does not specifically pertain to AWS scope of support. We however try to assist you with the best of our knowledge and efforts towards understanding your concern and point towards right direction.

```
https://aws.amazon.com/premiumsupport/faqs/#Third-party_software 
```

Moving further, I was researching more on the concern. I can see that the Zabbix is more compatible with CentOS 8 / RHEL 8 / Oracle Linux 8 / Ubuntu 20.4. Please refer below article for more details.

[How to Install Zabbix 6.0 on CentOS 8 [Step-by-Step]](https://bestmonitoringtools.com/how-to-install-zabbix-server-on-centos-or-rhel/)

I will try to replicate in my test environment and will get back to you if I found something relevant details which could help you.

Please let me know in case you have any further issue or query. I will be more than happy to assist you further.

We value your feedback. Please share your experience by rating this correspondence using the AWS Support Center link at the end of this correspondence. Each correspondence can also be rated by selecting the stars in top right corner of each correspondence within the AWS Support Center.

Best regards,
Vivek G.
Amazon Web Services

====================

## Question to Zabbix:

Hello,

I am planning to use Zabbix 6.0 on AMZ linux 2. I want to ask compatibility of Zabbix 6.0 on AMZ Linux 2.

Detail about AMZ Linux 2:

```
cat /etc/os-release

NAME=”Amazon Linux”
VERSION=”2″
ID=”amzn”
ID_LIKE=”centos rhel fedora”
VERSION_ID=”2″
PRETTY_NAME=”Amazon Linux 2″
ANSI_COLOR=”0;33″
CPE_NAME=”cpe:2.3:o:amazon:amazon_linux:2″
HOME_URL=”https://amazonlinux.com/”

rpm -E %{rhel}
7
```

## Answer from Zabbix team:

Hello Yen,

Thank you for reaching out.

Please be informed that since AM2 is based on RHEL, most probably you will meet dependency issues, since we do not support Zabbix server 6 for RHEL7 only for 8.

Maybe you could try AL2022 instead, but we have no experience with it yet, so it would be nice if you can share the results with us.

Kind regards,

Antons

## More: 
According to this post, it may be installed but there is no assurance about working state: 
https://qiita.com/nakamura_trinet_co_jp/items/a1ace87c3b17a0ea3469

## Solutions:

・Option 1: Use an OS that is supported by Zabbix 6.0 such as Ubuntu 20.04, RHEL 8 [1]
・Option 2: Use Zabbix Cloud Images that Zabbix published at AWS Marketplace [2]
・Option 3: Use Docker Image of Zabbix 6.0 to launch Zabbix 6.0 Container [3]

Reference:

　[1] [Download and install Zabbix | Zabbix Packages](https://www.zabbix.com/jp/download?zabbix=6.0&os_distribution=ubuntu&os_version=20.04_focal)

　[2] [Download and install Zabbix | Zabbix Cloud Images](https://www.zabbix.com/cloud_images)

　[3] [Download and install Zabbix | Zabbix Containers](https://www.zabbix.com/container_images)

