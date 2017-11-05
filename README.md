
# Solution

This solution uses an Amazon CloudWatch event rule and an AWS Lambda function to trigger an AWS CodeBuild container to generate a backup of all AWS CodeCommit repositories within a particular AWS account and region. The backups consists of .tar.gz files (one for each repo) named using the repo name and a timestamp and store in an S3 bucket. One can use S3 lifecycle events to automatically move old backups into Amazon Glacier (cold storage) or alternatively specify an expiration for the backup files in S3 to have them deleted after a certain period of time. See the figure below for details.

![approach-overview](codecommit_backup_approach.png)

# Instructions

* Make sure the latest version of the [AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) is installed in your local box
* Make sure your AWS profile have permissions to create IAM roles, CloudWatch Event rules, CodeBuild projects and Lambda functions at a minimum
* Open script ./deploy.sh and update these parameters as desired: AWS profile, S3 buckets, and backup schedule 
* Important: CodeBuild requires the S3 bucket containing the backup scripts to reside within the same region as the CodeBuild build project
* By default, all CodeCommit repositories within the AWS region where the solution was deployed to will be backed up everyday at 2am UTC (cron(0 2 * * ? *)) into the S3 bucket specified
* Run script ./deploy.sh

```bash
  chmod +x ./deploy.sh
  ./deploy.sh
```

