##### NOTE: THIS ASSUMES LOGGING AGGREGATOR ACCOUNT IS ALREADY SETUP ######
##### REFER TO LOGGING AGGREGATOR SETUP README FILE FOR INSTRUCTIONS ######

----------------------------------------------------------------------------------------------------

### AVM - NEW Basic AWS Account Type ###

Account Vending Machine (AVM) Setup instructions and guide for setting up a Basic AWS Account Type.

----------------------------------------------------------------------------------------------------

Prerequisite:
1. Setup Logging Account for Log Aggregation . Follow "README-AVM-Logging-Account-Setup.md"

----------------------------------------------------------------------------------------------------

#-----------------------------------#
#     STAGE 2: CODE REPOs SETUP     #
#-----------------------------------#

Following are the steps to follow as a pre-req before running the AVM Code for each Account Type (e.g. Basic, Logging, Sandbox, etc). This is run only once to create code repos for each AWS Account Type.

Step 1: Update avm-basic-coderepo-setup.yaml template
*** Make sure the CodeRepo Names are accurate ****
 - Open template avm-basic-coderepo-setup.yaml and update the resource name parameters (IAMCodeCommitRepoName, LoggingCodeCommitRepoName, VPCCodeCommitRepoName, ChildPipelineCodeCommitRepoName) to meet the naming standard

Step 2: Login to AVM Account.

Step 3: Create CloudFormation Stack using avm-basic-coderepo-setup.yaml
  - Upload template to CloudFormation from your local computer 
  - StackName: "awsi-p-1-cavm-basic-coderepo-setup"

Following are the AWS resources deployed by this stack:
		1. IAMCodeCommitRepo
		2. LoggingCodeCommitRepo
		3. ChildPipelineCodeCommitRepo
		4. VPCCodeCommitRepo
		5. CodeCommitServiceRole and CodeCommitServicePolicy

Step 4: Capture the CMK Key ARN from Master Pre-req stack. This should be added to the CMK parameter in following files
Note:  This is a reminder in case this step was missed in Step 5 of the README-AVM-PreReq file. If this is not completed by this point, processes down the line will fail.
        1. "child-account-prereq-setup.yaml"
        2. "iam-baseline-roles.yaml"

Step 5: Upload CloudFormation templates to the appropriate AWS CodeCommit Repos created

			Pipeline-Repo Templates:  (example: AWSI-P-1-CAVM-BasicPipeline)
			1. avm-iam-baseline-pipeline.yaml
			2. avm-iam-custom-pipeline.yaml
			3. avm-iam-saml-pipeline.yaml
			4. avm-logging-pipeline.yaml
			5. avm-monitor-pipeline.yaml
			6. avm-vpc-pipeline.yaml
      7. Upload configuration file with the following naming format <AccountID>-configuration.json (e.g. 265312162544-configuration.json)

Note: Update the AVM Account ID parameter in all the templates before uploading
			IAM-Repo Templates: (example: AWSI-P-1-CAVM-BasicIAM)
			1. iam-baseline-roles.yaml
			2. iam-custom-roles.yaml
			3. iam-saml-setup.yaml
			4. iam-saml-update.yaml
			5. cusomer-managed-policies.yaml

Note: Update the AVM Logging Account ID parameter in all the templates before uploading
			Logging-Repo:  (example: AWSI-P-1-CAVM-BasicLogging)
			1. cloudtrail-child-setup.yaml
			2. config-child-setup.yaml
			3. vpc-flowlog-child-setup.yaml

      Upload all the Config Rules to the Logging-Repo: monitor-templates/*

Note: Upload VPC templates to AWSI-P-1-CAVM-BasicVPC
			1. avm-vpc-delete-default-all-regions.yaml

----------------------------------------------------------------------------------------------------
