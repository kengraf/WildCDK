# WildCDK
CDK implementation of WildRydes

## AWS CDK approach
Cloudshell environment fails due to the 1GB space limit

Set regional value for creating an instance.  Examples of my settings
[Ohio]()
[London]()

#### Cloudshell commands to create AWS EC2 instance to run the CDK
```
INSTANCE_ID=`aws ec2 run-instances --image-id $AMI --count 1 \
  --instance-type $TYPE --key-name $KEYNAME --security-group-ids $SG \
  --region $REGION --output text --query 'Instances[0].InstanceId'`
aws iam attach-role-policy --role-name FullAdminRole \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
aws iam create-instance-profile --instance-profile-name FullAdminRole
aws iam add-role-to-instance-profile --role-name FullAdminRole \
  --instance-profile-name FullAdminRole
aws ec2 associate-iam-instance-profile --instance-id $INSTANCE_ID \
  --iam-instance-profile Name=FullAdminRole
```

#### SSH to the newly created instance
```
sudo yum install -y git
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install node
node -e "console.log('Running Node.js ' + process.version)"
npm install -g aws-cdk
npm install -g aws-cdk-lib
npm install -g typescript


```

```
# clone solutions-contructs
git clone https://github.com/awslabs/aws-solutions-constructs.git
cd aws-solutions-constructs/source/use_cases/aws-serverless-web-app
# Set the proper version numbers in the package.json file
../../../deployment/align-version.sh
# Install dependencies
npm install
# Build the use case
npm run build

# Setup the CDK stack
cdk acknowledge 19179  
ACCTID=`aws sts get-caller-identity --query "Account" --output text`
REGION=`curl http://169.254.169.254/latest/dynamic/instance-identity/document|grep region|awk -F\" '{print $4}'`
cdk bootstrap $ACCTID/$REGION

# Deploy the use case
cdk synth
cdk deploy --all

```
The deploy command will push two stacks: S3StaticWebsiteStack and ServerlessBackendStack.  
The CloudFormation "S3StaticWebsiteStack" ouput "websiteURL" is the entry point to the app: https://'your-cf-id'.cloudfront.net

### Clean up
```
# SSH to EC2 instance when done, or use CloudFormation console
cdk destroy --all
```

```
# Use Cloudshell to take down the EC2 command instance
aws terminate-instances --instance-ids $INSTANCE_ID
```

