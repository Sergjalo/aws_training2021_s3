# Sub-task 1 – create a static web site

## 1. Create a static web site. 

1. Feel free to do anything you like, but keep in mind that the main goal is to have a lightweight folder with multiple files in it:
   a. a couple if interlinked HTMLs or an HTML page with some CSS styles is enough
   b. the site should not require any additional runtime environment like JVM or Node
   c. no backend is required
   d. you’ll have several other tasks dedicated to creation of a fully functioning web-application in the modules 3-8
   e. no heavy media resources (like large images/animations/videos) are recommended – you’ll have to upload the site to AWS multiple times

the static-website-example-master.zip ![static-website-example-master.zip] is created 
## Create an S3 bucket

2. Create an S3 bucket which name doesn’t include uppercase characters, includes your full name, and begins with a letter. Recommendation – choose a name generic enough so that the bucket may be reused for developing a web application later.

![get_pods.png](/arh/get_pods.png)

## Copy the static website

3. Copy the static website from step 1 to the bucket from step 2 using AWS CLI and named profile with appropriate permissions from the previous module.

## Enable static website hosting

4. Enable static website hosting on your S3 bucket and make sure that the content of your site is available via website endpoint of the bucket.

## Enable cross-region replication

5. Enable cross-region replication for the bucket from step 2.

aws configure list-profiles
aws s3 cp static-website-example-master s3://sergii-kotov-task01-static-website --profile FullAccessUserS3 --recursive
Invalid endpoint: https://s3.us_west-2.amazonaws.com

aws s3 cp static-website-example-master s3://sergii-kotov-task01-static-website --profile FullAccessUserS3 --recursive --region us-west-2

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

Action does not apply to any resource(s) in statement

{
    "Version": "2012-10-17",
    "Id": "Policy1631634559639",
    "Statement": [
        {
            "Sid": "Stmt1631634556247",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::sergii-kotov-task01-static-website/*"
        }
    ]
}
02_permission_for_public_access.png

it works
http://sergii-kotov-task01-static-website.s3-website-us-west-2.amazonaws.com/
03_websiteworks.png

Enable cross-region replication for the bucket from step 2.

Bucket Versioning- Enabled

sergii-kotov-task01-static-website-replication
04_replicated_new_file_in_image_folder.png

## task 2

bucket sergii-kotov-task01-3-vers creted with versioning enabled

aws s3api list-object-versions --bucket sergii-kotov-task01-3-vers --prefix file1.txt --query "Versions[?IsLatest].[VersionId]" --profile FullAccessUserS3

	aws s3api list-object-versions --max-items 2 --bucket $1 --prefix $2 --query "Versions[?LastModified<=$3][]" --profile FullAccessUserS3
	
from bash shell
./s3_get_versions.sh sergii-kotov-task01-3-vers file1.txt "'2021-09-22T07:21:49'"


## task 3

### 1
aws s3api list-objects-v2 --bucket sergii-kotov-task01-static-website --query "Contents[].[Key]" --profile FullAccessUserS3

### 2
 aws s3 cp ./ver1/file1.txt s3://sergii-kotov-task01-static-website --profile FullAccessUserS3

### 3

aws s3 cp ./ver1/file1.txt s3://sergii-kotov-task01-static-website --profile FullAccessUserEC2
aws s3 cp ./ver1/file1.txt s3://sergii-kotov-task01-static-website --profile FullAccessUserS3

aws s3api list-objects-v2 --bucket sergii-kotov-task01-static-website --profile FullAccessUserEC2

### 4
aws s3api list-objects-v2 --bucket sergii-kotov-task01-static-website --query "Contents[].[Key, Size]" --output table --profile FullAccessUserS3

