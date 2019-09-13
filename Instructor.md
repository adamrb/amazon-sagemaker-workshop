# Deep Learning with Sagemaker and Tensorflow

## Required IAM Roles and Permissions

### Sagemaker Role

This is a CloudFormation template for the IAM role that needs to be used by the students when creating their Sagemaker notebook.  It will output an ARN that will be used during the workshop.

Sagemaker Service Role (Cloudformation Template)

```
{
	"Resources": {
		"SageMakerLab": {
			"Type": "AWS::IAM::Role",
			"Properties": {
				"AssumeRolePolicyDocument": {
					"Version": "2012-10-17",
					"Statement": [{
						"Effect": "Allow",
						"Principal": {
							"Service": [
								"sagemaker.amazonaws.com"
							]
						},
						"Action": [
							"sts:AssumeRole"
						]
					}]
				},
				"ManagedPolicyArns": [
					"arn:aws:iam::aws:policy/AmazonSageMakerFullAccess"
				]
			}
		}
	},
	"Outputs": {
		"qwikLAB": {
			"Description": "Outputs to be used by qwikLAB",
			"Value": {
				"Fn::Join": [
					"", [
						"{",
						"\"Resource-ARN\": \"",
						{
							"Fn::GetAtt": [
								"SageMakerLab",
								"Arn"
							]
						},
						"\"}"
					]
				]
			}
		}
	}
}
```

### Sagemaker User Policy:
These are the IAM permissions that should be setup for every user.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:*",
                "ecr:GetAuthorizationToken",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "ecr:BatchCheckLayerAvailability",
                "cloudwatch:PutMetricData",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents",
                "logs:GetLogEvents",
                "s3:CreateBucket",
                "s3:ListBucket",
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:ListAllMyBuckets",
                "iam:ListRoles"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "sagemaker.amazonaws.com"
                }
            }
        }
    ]
}
```

## Resources Created
Below are the resources required in the AWS account that will be created.  All are per student unless noted otherwise

- 1 S3 Bucket
- 1 Sagemaker Role (can be shared by all students)
- Sagemaker
	- General
		- 1 Sagemaker Notebook instance (ml.m4.xlarge)
	- Module 2 (Gaming)
		- 1 Training instance (ml.c4.xlarge)
		- 1 Endpoint (ml.t2.medium)
	- Module 3 (Distributed TensorFlow)
		- 1 Training instance (2x ml.c4.8xlarge)
		- 1 Endpoint (ml.m4.xlarge)
	- Module 4 (Image Classification)
		- 1 S3 Bucket
		- 1 Training instance (ml.p2.8xlarge)
		- 1 Training instance (2x ml.c4.8xlarge)
		- 1 Endpoint (ml.m4.xlarge)
