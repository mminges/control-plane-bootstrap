# What Is Control Plane Bootstrap?
The intention of this repo is to provide a space for a first-startup of a Control Plane. This includes: 
* IaaS paving 
* Ops Manager creation
* Director Configuration
* Concourse Deployment

After first installing Concourse and its dependencies, all future changes/management is to be run via automation pipelines (both for new foundations and self-management of the Control Plane itself).

# Dependencies

## Runtimes
* om
* bosh
* aws-s3 or minio

## Director Configuration
Director is configured and has vm extensions added.

## VM Extensions
* concourse-lb
* increased-disk
* iam_instance_profile

## Ops Manager Env
Update env.yml template (needs password for admin or admin account) under appropriate environment directory or use existing file from elsewhere

# How to Install Me

## Get Tanzu Network Concourse tarball and releases
Download all files from Tanzu Network for Concourse 6.3.1 Platform Automation and place them in the respected S3 bucket location. [Link](https://network.pivotal.io/products/p-concourse/#/releases/717385)

## Get Tanzu Network Stemcell 621
Concourse 6.3.1 requires the Xenial Stemcell version 621.*. Please download the version for AWS, the name begins with 'light-' and is a yaml file then place in the respected S3 bucket location [Link](https://network.pivotal.io/products/stemcells-ubuntu-xenial/#/releases/787669)

## Determine Variables
Check environments directory for variable values, ensure they are correct and modify as necessary (ensure you commit changes)
Create new environments directory if creating new environment deployment 

## Determine Secrets
Put secrets in environments/(your_environment_name_here)/secrets.yml
Import this yaml file into credhub

## Run Script

> NOTE: \<IAAS\> should match the folders underneath either environments or templates. \<PRODUCT_VERSION\> should match the directory version name of the product you are trying to deploy.

Example Concourse deploy:
`./deploy_me aws 6.3.1` 

### Opsman:
> NOTE: For Air Gapped environments run:
export AIRGAPPED_CLOUD=true

```bash
cd scripts/opsman/
./deploy_me <IAAS> <PRODUCT_VERSION>
```

### Director:
> NOTE: For Air Gapped environments run:
export AIRGAPPED_CLOUD=true

```bash
cd scripts/director/
./deploy_me <IAAS> <PRODUCT_VERSION>
```

### Concourse:
> NOTE: For Air Gapped environments run:
export AIRGAPPED_CLOUD=true
export S3_BUCKET=<BUCKET_NAME>

```bash
cd scripts/concourse/
./deploy_me <IAAS> <PRODUCT_VERSION>
```
## Clean up after yourself
Commit changes to variables or the scripts.
Delete artifacts (stop taking up space you don't need).
Revert secrets.
