# Client Instructions

### Create a bucket for our CFT files
		aws s3api create-bucket --bucket singledigit-cft --profile singledigit
		
### Copy CFT files to bucket
		aws s3 sync ./cft s3://singledigit-cft --profile singledigit
	
### Create the basic stack
		aws cloudformation create-stack --stack-name pipeline-client-infra \
			--template-url=https://s3.amazonaws.com/singledigit-cft/base.yml \
			--profile singledigit
			
### Upload code to hosting bucket
		aws s3 sync ./src s3://<deploy-bucket> --acl 'public-read' --profile singledigit