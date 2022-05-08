There are three type of AVMs that deploys AWS Accounts of following types.
Type 1: AVM for Logging AWS Account
Type 2: AVM for Basic AWS Account (used for Prod and Non-Prod VPC based applications)
Type 3: AVM for SandBox AWS Account (used for development and test purposes)

Account Type: Account ID
------------------------
AVM: 037765247764
Logging: 539411578104
AMI Bakery: 265312162544

#------------------------------------#
# STAGE 1: AVM ACCOUNT PRE-REQ SETUP #
#------------------------------------#

Step 1: Update template "avm-account-prereq-setup.yaml"
- Note: This first step is done only once per AVM Account and one AVM Account will have more than one Account Vending Machine (AVM); AVM for Basic, AVM for Logging, AVM for Sandbox
- Open template "avm-account-prereq-setup.yaml" and update all the resource name parameters to meet the naming standards and AVM type

Step 2: Upload template to S3 Bucket for pre-req templates (Bucket Name: awsi-p-1-cavm-s3-prereq) in AVM Account

Step 3: Deploy CloudFormation Stack with AVM Pre-Req parameters template "avm-account-prereq-parameters.yaml"
		StackName: "awsi-p-1-cavm-account-prereq-parameters"
		Region: Deploy in Oregon (us-west-2)

Step 4: Deploy CloudFormation Stack with AVM Pre-Req template "avm-account-prereq-setup.yaml"
		StackName: "awsi-p-1-cavm-account-prereq-setup"
		Region: Deploy in Oregon (us-west-2)

Step 5: Review deployment
Following are the resources deployed by this stack:
		1. SNS Topic: CodePipelineSNSTopic
		2. KMS Key: AVMKMSKey and AVMKMSKeyAlias
		3. S3 Bucket: ADMetadataBucket
		4. S3 Bucket: ArtifactStoreBucket
		5. IAM Role and Policy: AVMMasterCodePipelineServiceRole and AVMMasterCodePipelineServicePolicy
		6. IAM Role and Policy: CloudFormationChildPipelineDeployerRole and CloudFormationChildPipelineDeployerPolicy
		7. IAM Role and Policy: AVMChildCodePipelineServiceRole and AVMChildCodePipelineServicePolicy
		8. CodeCommit repository: AVMCodeCommitRepo
		9. IAM Role: InvokePipelineCPServiceRole

Step 6: Commit Lambda functions and setup templates into AVMCodeCommitRepo CodeCommit repository
  1. Copy and commit entire "Lambda" directory.
	2. Copy and commit avm Pre-Req-Templates directory.

Step 7: Provision Invoke_Pipeline Lambda function
  Deploy cloudformation stack with lambda pipeline template "buildlambda_codepipeline.yaml" found in Lambda/Invoke_Pipeline/Stack directory.
	Deployment option:  Choose to upload file.
	StackName: awsi-p-1-cavm-BuildInvokePipelineLambdaCP

Step 8: Configure Identity Provider and Upload AVM Account MetaData File
  - In IAM Service, select Identity Provider
	  - Provider Type = SAML
		- Provider Name ='AzureADSync'
		- Upload the AD Metadata file with filename format "AzureADMetadata-<AccountID>.xml"
	- Note:  this is a one time manual entry

Step 9: From Master Pre-req stack, capture the S3 Bucket ARN and CMK Key ARN for all the deployed regions. These should be added to the following files:
    1. "child-account-prereq-setup.yaml"
    2. "iam-baseline-roles.yaml"
NOTE: If the CMK Key is rotated in the future then, all then the above templates should be redeployed with the new KMS ARN

NOTE: Based on the type of account to be setup, either proceed to the README-AVM-Logging.md file to setup a Logging account, otherwise, proceed to the README-AVM-Basic.md file to setup a Basic account.
