# CodeBuild Automation Test

Option A: Use the GitHub Quick Start in EventBridge (Easiest)
AWS provides a native inbound partner integration that automatically provisions an endpoint for GitHub without needing API Destinations.
1. In the EventBridge Console, go to Buses > Partner event sources.
2. Click Set up and look for GitHub in the partner list.
3. Follow the prompt to log into GitHub and authorize the AWS Integration App.
4. AWS will automatically generate a Partner Event Source ARN and handle the underlying URL infrastructure. You won't have to manually paste a URL into GitHub; the GitHub App handles the handshake directly.

# Errors

1. ACCESS_DENIED: Service role arn:aws:iam::327573816970:role/service-role/codebuild-aws-code-build-1-service-role does not allow AWS CodeBuild to create Amazon CloudWatch Logs log streams for build arn:aws:codebuild:us-east-1:327573816970:build/aws-code-build-1:efd74f83-30dd-4fdd-b82f-e6269386ef40. Error message: User: arn:aws:sts::327573816970:assumed-role/codebuild-aws-code-build-1-service-role/AWSCodeBuild-efd74f83-30dd-4fdd-b82f-e6269386ef40 is not authorized to perform: logs:CreateLogStream on resource: arn:aws:logs:us-east-1:327573816970:log-group:/aws/codebuild/aws-code-build-1:log-stream:efd74f83-30dd-4fdd-b82f-e6269386ef40 because no identity-based policy allows the logs:CreateLogStream action

Solution: Step-by-Step Fix1. Locate the IAM RoleOpen the AWS IAM Console.In the left navigation menu, click Roles.In the search box, paste your exact role name: codebuild-aws-code-build-1-service-roleClick on the role name from the search results to open its settings.2. Add the Missing CloudWatch PolicyOn the Permissions tab, click Add permissions on the right side, then select Attach policies.Click Create policy (this opens a new browser tab).Switch from the visual editor to the JSON tab and paste the following policy:json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:us-east-1:327573816970:log-group:/aws/codebuild/aws-code-build-1",
        "arn:aws:logs:us-east-1:327573816970:log-group:/aws/codebuild/aws-code-build-1:*"
      ]
    }
  ]
}
