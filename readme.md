# Sub-task 1 – create a static web site

## 1. Create a static web site. 
```
1. Feel free to do anything you like, but keep in mind that the main goal is to have a lightweight folder with multiple files in it:
   a. a couple if interlinked HTMLs or an HTML page with some CSS styles is enough
   b. the site should not require any additional runtime environment like JVM or Node
   c. no backend is required
   d. you’ll have several other tasks dedicated to creation of a fully functioning web-application in the modules 3-8
   e. no heavy media resources (like large images/animations/videos) are recommended – you’ll have to upload the site to AWS multiple times
```

the [static-website-example-master.zip](static-website-example-master.zip) is created 

## Create an S3 bucket
```
2. Create an S3 bucket which name doesn’t include uppercase characters, includes your full name, and begins with a letter. Recommendation – choose a name generic enough so that the bucket may be reused for developing a web application later.
```
**sergii-kotov-task01-static-website**

## Copy the static website

```
3. Copy the static website from step 1 to the bucket from step 2 using AWS CLI and named profile with appropriate permissions from the previous module.
```
**aws s3 cp static-website-example-master s3://sergii-kotov-task01-static-website --profile FullAccessUserS3 --recursive --region us-west-2**
![01_upload.png](/01_upload.png)

## Enable static website hosting

```
4. Enable static website hosting on your S3 bucket and make sure that the content of your site is available via website endpoint of the bucket.
```

important detail here is to add /* to the resource ARN.
because for the permissions like this 
```
{
    "Version": "2012-10-17",
    "Id": "Policy1631634559639",
    "Statement": [
        {
            "Sid": "Stmt1631634556247",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::sergii-kotov-task01-static-website"
        }
    ]
}
```
there is error `Action does not apply to any resource(s) in statement`

So here is the correct one:

![02_permission_for_public_access.png](/02_permission_for_public_access.png)

Static site works:
[http://sergii-kotov-task01-static-website.s3-website-us-west-2.amazonaws.com/](http://sergii-kotov-task01-static-website.s3-website-us-west-2.amazonaws.com/)
![03_websiteworks.png](/03_websiteworks.png)

## Enable cross-region replication

5. Enable cross-region replication for the bucket from step 2.

Bucket Versioning- Enabled

Bucket `sergii-kotov-task01-static-website-replication` created

![04_replicated_new_file_in_image_folder.png](/04_replicated_new_file_in_image_folder.png)

# Sub-task 2 – play with versioning

## Create another S3 bucket

1. Create another S3 bucket which name doesn’t include uppercase characters, includes your full name + “task2”, and begins with a letter.
2. Enable versioning for the bucket from step 1.

bucket sergii-kotov-task01-3-vers creted with versioning enabled
![](/)
## Upload & Change

3. Upload 2-3 files to the bucket. Make some changes to these files so that the bucket contains 2 (or more) versions of at least one file.

2 files uploaded from `\ver1` and then another 2 fiiles with same names from `\ver2` forlder.

## AWS CLI get the latest 

4. Using AWS CLI, get the latest version of a specific file.

aws s3api list-object-versions --bucket sergii-kotov-task01-3-vers --prefix file1.txt --query "Versions[?IsLatest].[VersionId]" --profile FullAccessUserS3
![05_versioning.png](/05_versioning.png)

## Script to get the latest 

5. Optional: write a script to get the latest version of a specific file no newer than a given date. You are free to use Bash or BAT or use the AWS SDK for any programming language

the [s3_get_versions.sh](/s3_get_versions.sh) script is created

launched from bash shell as 

*./s3_get_versions.sh sergii-kotov-task01-3-vers file1.txt "'2021-09-22T07:21:49'"*

![06_versioning_cli.png](/06_versioning_cli.png)


## task 3

### 1

aws s3api list-objects-v2 --bucket sergii-kotov-task01-static-website --query "Contents[].[Key]" --profile FullAccessUserS3
![07_list_keys.png](/07_list_keys.png)

### 2

aws s3 cp ./ver1/file1.txt s3://sergii-kotov-task01-static-website --profile FullAccessUserS3
![.png](/.png)

### 3

aws s3 cp ./ver1/file1.txt s3://sergii-kotov-task01-static-website --profile FullAccessUserEC2
aws s3 cp ./ver1/file1.txt s3://sergii-kotov-task01-static-website --profile FullAccessUserS3

aws s3api list-objects-v2 --bucket sergii-kotov-task01-static-website --profile FullAccessUserEC2

![08_upload_with_different_profiles.png](/08_upload_with_different_profiles.png)

### 4
aws s3api list-objects-v2 --bucket sergii-kotov-task01-static-website --query "Contents[].[Key, Size]" --output table --profile FullAccessUserS3

![09_list_with_different_profile.png](/09_list_with_different_profile.png)

