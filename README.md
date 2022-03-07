# WildCDK
CDK implementation of WildRydes

## AWS CDK appraoch
Cloudshell environment fails due to the inability to use spawn
Debian instructions
```
sudo -i
yum install git
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
. ~/.nvm/nvm.sh
nvm install node
node -e "console.log('Running Node.js ' + process.version)"
npm install aws-cdk
npm install typescript
aws configure
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
# Deploy the use case
cdk deploy

```
