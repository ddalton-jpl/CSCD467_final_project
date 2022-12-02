# aws-thumbnail-s3-lambda #
Sample Code for generate a thumbnail on s3 using lambda and python


## How to compile and deploy ##

##### Create 2 S3 buckets. 1 for the original image and 1 for the thumbnail #####

##### Create S3 and lambda roles with this policy #####
```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:CreateLogStream"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "<arn s3 source>/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "<arn s3 dest/*"
        }
    ]
}
```

### Create Function

```sh
aws lambda create-function --function-name CreateThumbnail \
--zip-file fileb://my-deployment-package.zip --handler lambda_function.lambda_handler --runtime python3.7 \
--timeout 10 --memory-size 1024 \
--role <s3 role arn>
```


### Update Function Code

```sh
zip -g my-deployment-package.zip lambda_function.py
aws lambda update-function-code --function-name CreateThumbnail --zip-file fileb://my-deployment-package.zip
```

### Package Python Function

```sh
virtualenv myenv
source myenv/bin/activate
pip install Pillow
pip show Pillow
deactivate
cd myenv/lib/python3.7/site-packages
zip -r my-deployment-package.zip .
mv my-deployment-package ~/environment
zip -g my-deployment-package.zip lambda_function.py
```