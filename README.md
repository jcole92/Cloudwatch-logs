# Cloudwatch
# 1. How to Create Lambda Functions to use as Logs For CloudWatch Alarms
Hi, for this project I walk though the steps of creating a Lambda function to use as a Cloudwatch Log. I use that log to create a CloudWatch alarm to test the spikes of the Lambda function. 

1) This CloudFormation template uses native AWS CloudFormation resources (AWS::Logs::LogGroup, AWS::Lambda::Function, AWS::Logs::MetricFilter) without requiring external libraries. The template version is "2010-09-09" which is the standard CloudFormation version.

2) AWS resources impacted: CloudWatch Logs Log Groups (Demo-logs, /aws/lambda/HelloWorld), AWS Lambda Function (HelloWorld), CloudWatch Logs Metric Filter (DemoFilter).

3) The code creates a CloudWatch log group named "Demo-logs" with STANDARD class, creates a Lambda function named "HelloWorld" with Python 3.12 runtime, creates an associated log group for the Lambda function, and sets up a metric filter named "DemoFilter" on the Lambda log group to track metrics in the "DemoNameSpace" namespace.

4) The template defines four main resources: DemoLogsLogGroup for general logging, HelloWorldLambdaFunction with inline Python code that processes key-value pairs from events, HelloWorldLambdaLogGroup for Lambda execution logs, and DemoMetricFilter that captures all log events with an empty filter pattern. The Lambda function uses 128MB memory, 3-second timeout, and x86_64 architecture.

5) The Lambda invocations from the CLI commands are runtime operations and cannot be represented in CloudFormation as they are execution actions, not infrastructure definitions. CloudFormation creates and configures resources but does not invoke them.

6) The metric filter is configured with an empty filter pattern (matching all log events) and transforms each matched event into a metric value of 1 in the DemoCloudwatchLog metric within the DemoNameSpace namespace.

7) Resource dependencies are properly set using DependsOn to ensure the Lambda function is created before its log group, and the log group exists before the metric filter is applied.

8) All numeric values (Timeout, MemorySize, RetentionInDays, MetricValue) are converted to strings as required by JSON format specifications,