# terraform-moto-example

This examples demonstrates how `terraform` infrastructure can be created against `moto`.

## Running the example

1. Clone the repository:

```bash
git clone https://github.com/paololazzari/terraform-moto-example .
cd terraform-moto-example
```

2. Start the docker container moto backend:

```bash
docker run --rm -it -p 5000:5000 --name moto_backend -d motoserver/moto:latest
```

3. Configure fake AWS credentials:

```bash
export AWS_CONFIG_FILE=$(pwd)/config.txt
export AWS_SHARED_CREDENTIALS_FILE=$(pwd)/creds.txt
export AWS_PROFILE=demo
```

4. Run terraform apply:

```bash
terraform init
terraform apply -auto-approve
```
5. Run an AWS CLI command against the moto server to verify the creation of the resource:

```bash
aws ec2 describe-instances --region us-east-1 --endpoint-url http://localhost:5000
```

## Cleaning up

1. Teardown terraform:

```bash
terraform destroy -auto-approve
```

2. Kill moto docker container:

```
docker rm -f moto_backend
```

3. Unset fake AWS credentials:

```bash
unset AWS_CONFIG_FILE
unset AWS_SHARED_CREDENTIALS_FILE
unset AWS_PROFILE
```
