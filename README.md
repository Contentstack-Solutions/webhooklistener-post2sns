# AWS Lambda Webhook Listener - Posts AWS SNS
## Based on a similar script, webhooklistener-post2Slack
## POC in Python

Using Python 3.8 on [AWS Lambda](https://aws.amazon.com/lambda/).

1. Create a Lambda function and an API Gateway in AWS Lambda. Based on these documentation articles:
   * [TUTORIAL: Build an API Gateway API with Lambda Non-Proxy Integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-lambda-non-proxy-integration.html).
   * Note: You will need to pass custom headers through the API gateway to the Lambda function for it to work. Exactly like it's done in [this article](https://aws.amazon.com/premiumsupport/knowledge-center/custom-headers-api-gateway-lambda/).
   * We recommend enabling authentication on the API endpoint.
   * [AWS Lambda Deployment Package in Python](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html)
   * (Optional) Create tests in Lambda, using the contents of the `LambdaTests` folder.

2. Create a SNS Topic. Define a subscription in that Service to chose the delivery of the message:
  * SMS
  * Email
  * Slack/MS Teams or something else. [See this article from AWS](https://aws.amazon.com/premiumsupport/knowledge-center/sns-lambda-webhooks-chime-slack-teams/)


    Read more about SNS here: https://aws.amazon.com/sns/
    Read more on SNS/Python here: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sns.html#SNS.Client.publish.

3. Environmental variables needed:
  * SNS Topic ARN - `SNS_TOPIC_ARN`


4. It's good to update both code and configuration in Lambda with `awscli`.
   * To update code via `awscli`:
    * install pip modules into a subdirectory like this:
      * `pip install --target ./package requests`.
      * `pip install --target ./package boto3`
    * Add all files and folders from the `package` folder (the files and folders from the `package` folder, not the `package` folder itself) into a zip file with the `lambda_function.py`, e.g like this:
      1. `zip -g function.zip lambda_function.py`
      2. `cd packages`
      3. `zip -g ../function.zip *`
      4. `cd ..`
    And finally upload to Lambda like this:
      * `aws lambda update-function-code --function-name WebhookListener-SNS --zip-file fileb://function.zip`

5. Create a webhook in Contentstack, pointing to the API Gateway defined in step 1.
    * Currently supports update (save), publish, unpublish, delete and changing workflow stages on entry level.
