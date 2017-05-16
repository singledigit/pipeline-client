# Client Instructions

## Local Development

#### Create a bucket for our CFT files
	aws s3api create-bucket --bucket singledigit-cft --profile singledigit
		
#### Copy CFT files to bucket
	aws s3 sync ./cft s3://singledigit-cft --profile singledigit
	
#### Create the basic stack
	aws cloudformation create-stack --stack-name <stack-name> \
		--template-url=https://s3.amazonaws.com/singledigit-cft/base.yml \
		--profile singledigit
		
#### Upload code to hosting bucket
	aws s3 sync ./src s3://<deploy-bucket> --acl 'public-read' --profile singledigit
	
## CICD
#### Create bucket in new environment
	aws s3api create-bucket --bucket singledigit-demo-cft --profile demo
	
#### Copy CFT files to bucket
	aws s3 sync ./cft s3://singledigit-demo-cft --profile demo	
	
#### Creating the CICD pipeline
	aws cloudformation update-stack \
		--profile demo \
		--stack-name pipeline-client-cicd \
       --template-url=https://s3.amazonaws.com/singledigit-demo-cft/cicd.yml \
       --capabilities=CAPABILITY_NAMED_IAM \
       --parameters ParameterKey=GitHubOwner,ParameterValue=singledigit \
       ParameterKey=Repo,ParameterValue=pipeline-client \
       ParameterKey=Branch,ParameterValue=master \
       ParameterKey=Token,ParameterValue=