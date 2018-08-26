---
layout: post
title:  "Connecting to multiple AWS accounts using AWS CLI"
date:   2017-03-08 07:55:09 +0530
---
To connect to multiple AWS accounts using different IAM users, following are the AWS CLI configuration steps.

```
$ aws configure --profile
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: <region, default is us-east-1>
Default output format [None]: <text, json or table; default is json>
```

This would add _user2_ in the config file.

```
$ more ~/.aws/config
[default]
region = us-east-1
output = JSON
[profile user2]
region = us-east-1
```

Note that the credentials are stored in the credentials file.

```
$ more ~/.aws/credentials
[default]
aws_access_key_id = ***
aws_secret_access_key = ***
[user2]
aws_secret_access_key = ***
aws_access_key_id = ***
```
To connect to the first AWS account using the default profile:

```
$ aws s3 ls
2017-03-02 07:23:57 bucket1
2017-03-02 07:37:16 bucket2
2017-03-02 07:39:17 bucket3
```

To connect to the second AWS account using _user2_ profile:

```
$ aws s3 ls --profile user2
2017-03-04 09:43:03 bucketA
2017-03-04 10:11:18 bucketB
```

More detailed instruction [here](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html).