## Project of deployment on AWS

Responsible of deploying the jewelry project on AWS

## Main technologies

 - IaC: Terraform
 - Cloud: AWS (EC2, S3, DynamoDB)
 - CI/CD: GitHub Actions
 - Build: Make
 - Security: Checkov & Trivy

## Make commands

### Automated deploy on AWS
```bash
make aws-deploy
```

### Manual deploy

```bash
# Initialize the environment
make init
# Plan the infrastructure
make plan
# Apply the infrastructure
make apply
``` 
or
```bash
# Applies the infrastructure
make deploy
```

### Usefull commands

```bash
# Builds the application
make build

# Cleans temporary files
make clean

# Destroy any environment already running
make aws-destroy
```

Builds and run on docker:
```bash
make docker-run
```

## Pipeline and Deploy

This repository has a CI pipeline that is responsible to the integrity check and deployment.

 - The pipeline needs the **AWS key** activated to work

### What does the pipeline do?

1. **Security scan**: First it runs the Checkov to check the infrastructure that is being created with terraform and Trivy to scan the project and look for security issues. Right now both are in the mode "soft fail", that they only report the errors, but don't fail the pipeline.
2. **Destroy**: In sequence, it executes "make aws-destroy" to clean any old infrastructure related to this project.
3. **Apply**: Then the pipeline executes "make aws-deploy" to create all the infrastructure the project needs to run with new changes.

The pipeline grants that every update on main is tested, destroyed and recreated all automated.

### Security
To grant better security for the project, Checkov and Trivy scans are used for static and container analysis.

Right now they are configured to accept errors with the "soft fail" config, to grant functionality and deployment on AWS.

## AWS infrastructure
 - Ec2 instance with Docker
 - Subnet
 - Security Group

## Accessing the application
After the end of the pipeline, in the terraform outputs it's going to show the app url

Outputs:
```bash
app_url = "APP: http://<EC2_PUBLIC_IP>:8080"
```
## Main questions

### Can the project created for the OLD client technically be implemented in the NEW client's environment?
Yes, changing the infrastructure that terraform creates from Azure to AWS.

### Which items need to be adjusted? IaC? Docker? Makefile?
IaC and Makefile needs to be ajusted to work on AWS, because they have the Azure semantics.
