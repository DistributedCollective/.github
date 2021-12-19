# .github
https://docs.github.com/en/actions/learn-github-actions/creating-workflow-templates


Github Workflow Template:
https://github.com/DistributedCollective/.github/blob/master/workflow-templates/ci-cd-development.yml

# Template use several reusable workflows, namely:

1. init.yml (generate workflow variables from ci-properties.json)
2. docker.yml (build and push docker image to registry)
3. deploy-k8s.yml (login and deploy application to EKS)

# Required repo/org secrets:
IAM credentials with access to EKS cluster("cluster_name" propertie in ci-properties.json) and 'production' and 'test' namespaces

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

# Credentials for docker registry ("registry" property in ci-properties.json)

DOCKER_USERNAME

DOCKER_PASSWORD

# How to:
1. Ensure secrets in repo/org (see #Required repo/org secrets)
2. Push workflow template to repo and define 'branches: []' parameter with branches which should support CI workflow
3. Create deployment.yaml in root of repository
4. Add ci-properties.json file to .github/workflow/ci-properties.json
```
{
  "app_name": "safe-transaction-service", #  name of k8s deployment etc
  "aws_region": "us-east-2", # AWS region for aws login command and auth to EKS
  "k8s_cluster_name": "k8-mainnet", # EKS cluster name in region
  "registry": "docker.io", # Docker registry name
  "image_name": "sovryn/safe-transaction-service", # Docker image name
  "prod_branch": "rsk", # branch which will be deployed to production k8s namespace
  "dev_branch": "develop", # branch which will be deployed to test k8s namespace
  "dockerfile_path": "./docker/web", # path to Dockerfile in repo
  "APP_ENV_VARS": { # Application envriroment variables, will be passed as enviroment variables inside application pod
    "PYTHONPATH": "/app/",
    "DEBUG": "0",
    "ETH_L2_NETWORK": "1",
    "ETH_INTERNAL_NO_FILTER": "1"
  }
}
```
5. Create k8s secret for this application if necessary (name of the secret must be defined in deployment.yaml) 
