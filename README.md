# promote_to_production

This repo defines a set of jobs that promotes a new environment to production and decommissions the old environment in an automated process.

## TOOLS

- AWS
- AWS S3
- Ansible
- Circle CI

## STEPS

### S3 BUCKET

You can find a more detailed description of how to create and configure a bucket for static website hosting (here)[https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html]

But basically, you'll need to folow these steps:

- Create a (S3 bucket)[https://s3.console.aws.amazon.com/]
- Upload the index_v1.html file to the bucket
- Enable static website hosting
- Edit Block Public Access settings (Disable the Public Access Setings)
- Add Bucket Policy

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}
```

```
aws cloudformation deploy --template-file cloudfront.yml --stack-name production-distro --parameter-overrides PipelineID="${S3_BUCKET_NAME}" --tags project=udapeople &
```
