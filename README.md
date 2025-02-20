# WordPress frontend proxy
A small app that uses the WordPress API to load content and serve a site.  The pages are rendered using handlebars.

Also includes the Terraform to run this as a Lambda function behind a CloudFront distribution.

## Running locally
```sh
cd app
cp .env.example .env # and set your values
npm install
npm start
```

## Terraform
You will need to build a Docker image to run, which is currently exected to be an `arm64` image.
```sh
docker build \
    --tag wordpress-frontend \
    --file ./app/Dockerfile \
    --platform linux/arm64 ./app 
```

You can do it as `amd64` as well if you update the `terraform/lambda.tf` resource's [architectures](https://github.com/patheard/wordpress-frontend/blob/dfa9dfc6d26b62396fdb5f00b8c2e1dd4d615c59/terraform/lambda.tf#L7).

```sh
cd terraform
terraform init
terraform apply
```

The above will fail on first run since the Docker image will not be in the new Elastic Container Registry (ECR).  Push your Docker image up and re-run the apply to fix.
