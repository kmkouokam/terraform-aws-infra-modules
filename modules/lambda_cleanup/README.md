# Lambda Cleanup Submodule

This Terraform submodule provisions a scheduled AWS Lambda function that automatically cleans up unused EC2 resources such as unassociated EBS volumes, idle EC2 instances, and unattached Elastic IPs. It also configures the necessary IAM roles, SNS alerts, and a CloudWatch Events rule for weekly execution.

---

## Features

- **Weekly Cleanup**: Scheduled every Monday at 00:00 UTC via CloudWatch Events.
- **Permissions**: IAM role with policies to describe and delete EC2 resources, publish SNS alerts, and write logs.
- **Alerting**: Notifies via the provided SNS Topic ARN when actions are performed.
- **Environment Tagging**: Resources are tagged using the `env` variable.
- **Logging**: Sends logs to CloudWatch Logs.
- **Python Runtime**: Uses Python 3.11 and expects a zipped Lambda package at `function.zip`.

---

## Usage

```hcl
module "lambda_cleanup" {
  source             = "github.com/kmkouokam/infra-modules//aws/modules/lambda_cleanup"
  env                = "prod"
  aws_sns_topic_arn  = aws_sns_topic.alerts.arn
}
```

Ensure that a valid `function.zip` is available in the module directory, containing your Python handler with the entrypoint `lambda_function.lambda_handler`.

---

## Input Variables

- `env` – (string) Environment label used in naming/tagging. Default: `"dev"`.
- `aws_sns_topic_arn` – (string) The ARN of the SNS topic where cleanup notifications will be sent.

---

## Outputs

- `lambda_function_name` – Name of the Lambda function.
- `lambda_function_arn` – ARN of the Lambda function.

---

## Schedule Details

The cleanup function runs on the following schedule:

- `cron(0 0 ? * 1 *)` — every Monday at 00:00 UTC.

---

## Example Use Cases

- Automatically delete unattached EBS volumes to reduce costs.
- Terminate stopped EC2 instances that have been idle for too long.
- Reclaim Elastic IPs that are not associated with any instance.

---

## Notes

- Extend the function logic if needed to clean up additional AWS resources.
- Be cautious with the IAM permissions and limit scope in production.
- The function sends logs to CloudWatch and alerts to the SNS topic for observability.
 