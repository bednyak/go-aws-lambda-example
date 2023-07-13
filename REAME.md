# Go-AWS-Lambda-example

### How to run Lamdba

1. Install AWS-Cli
2. Set AWS-Cli global config on your machine
3. Run next command to create new role for lambda
```
aws iam create-role --role-name lambda-ex --assume-role-policy-document '{"Version": "2012-10-17", "Statement": [{ "Effect": "Allow", "Principal": { "Service": "lambda.amazonaws.com" }, "Action": "sts:AssumeRole" }] }'
```

#### OR

1. Create 'trust-policy.json' file
2. Add next code to 'trust-policy.json'
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
3. Run next command for pointing to 'trust-policy.json' file
```
aws iam create-role --role-name lambda-ex --assume-role-policy-document file://trust-policy.json
```

### After creating role and policies

1. Build Go project
```
go build main.go
```
2. Zip main binary file
```
zip function.zip main
```
3. Run command for creating lambda function
```
aws lambda create-function --function-name go-aws-lambda-example --zip-file fileb://function.zip --handler main --runtime go1.x --role arn:aws:iam::[YOUR_ID]:role/lambda-ex
```

### Finish

Now we can invoke this lambda function with next command

```
aws lambda invoke --function-name go-aws-lambda-example --cli-binary-format raw-in-base64-out --payload '{"What is your name?": "Jim", "How old are you?": 29}' output.txt
```