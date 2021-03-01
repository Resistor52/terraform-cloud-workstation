# CLI Kung Fu

This is just a list of AWS CLI commands that may come in handy. Listed here just to
streamline the presentation.

## Delete the Tech Tuesday IAM User

```
aws iam delete-user --user-name tech-tuesday
```

## Delete the cloud_workstation in AWS

```
aws ec2 delete-key-pair --key-name cloud_workstation
```

## List the AWS Access Keys

```
aws iam list-access-keys --user-name tech-tuesday --query AccessKeyMetadata[0].AccessKeyId --output text

```

## Disable the AWS Access Key (without deleting it)

```
aws iam update-access-key --access-key-id $(aws iam list-access-keys --user-name tech-tuesday --query AccessKeyMetadata[0].AccessKeyId --output text) --status Inactive --user-name tech-tuesday
```

## Delete the AWS Access Key

```
aws iam delete-access-key --access-key-id $(aws iam list-access-keys --user-name tech-tuesday --query AccessKeyMetadata[0].AccessKeyId --output text) --user-name tech-tuesday
```

# Rotate AWS Access Key during demo

```
aws iam delete-access-key --access-key-id $(aws iam list-access-keys --user-name tech-tuesday --query AccessKeyMetadata[0].AccessKeyId --output text) --user-name tech-tuesday && SECRET=$(aws iam create-access-key --user-name tech-tuesday | jq '.AccessKey.SecretAccessKey') && KEY=$(aws iam list-access-keys --user-name tech-tuesday --query AccessKeyMetadata[0].AccessKeyId --output text) && echo -e "[myterraform]\naws_access_key_id = $KEY\naws_secret_access_key = ${SECRET:1:-1}" > ~/.aws/credentials
```

# Rotate PEM During Demo
```
aws ec2 delete-key-pair --key-name cloud_workstation && rm -f ~/cloud_workstation.pem && aws ec2 create-key-pair --key-name cloud_workstation --query 'KeyMaterial' --output text > ~/cloud_workstation.pem && chmod 400 ~/cloud_workstation.pem
```
