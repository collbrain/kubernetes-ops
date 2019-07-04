The easier way
================
This is not exactly the easy way but way easier than [the-manual-way.md](the-manual-way.md).

This methods walks you through how to create the VPC and a Kops cluster via
the scripts we provide instead of using the CLIs manually.  However, if you wanted
to learn what exactly it is doing and we encourage you to go through the [the-manual-way.md](the-manual-way.md)
once sometime.

# Replace all of the S3 buckets used
See [s3 buckets replacement](README.md#Setting up the S3 buckets)

# Create VPC

## Run

```
cd ops
./vpc.sh -n dev --create true --dry-run false
```

# VPC ID
From the output of the Terraform run, a VPC ID was outputted in the format of
`vpc-xxxxxxx`.  Copy this ID, you will need to put it into a few places.

The following paths all starts from the root of this repository.

## Terraform environment \_env_defaults file
This file hold default values about this environment.  We are adding in the
VPC ID here because there will be subsequent Terraforms that will use this ID
and place itself into this VPC.  

An example is if we wanted to use an RDS database.  We will put the database
in this VPC and it will need the VPC ID to do that.

File: `./tf-environments/dev/_env_defaults/main.tf`

Update the `vpc_id` variable with the ID.

## Kops values.yaml
This file holds the configuration for our Kops Kubernetes cluster for the `dev`
environment.  We are going to tell Kops to put itself into this VPC.

File: `./clusters/aws/kops/clusters/dev/values.yaml`

Replace the value of `vpc` with the VPC ID.

# Create the Kops Kubernetes cluster

Run:
```
cd clusters/aws/kops
./kops.sh --name dev --create true --dry-run false
```