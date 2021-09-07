# hello function using nix and serverless

## Development environment using nix

I created a folder, and use the following `default.nix`:

```nix
with import <nixpkgs> { };
stdenv.mkDerivation rec {
  name = "env";
  env = buildEnv {
    name = name;
    paths = buildInputs;
  };
  buildInputs = [ nodejs nodePackages.serverless awscli ];
}
```

Afterwards, I entered a shell (using `nix-shell`) and checked I had NodeJS, Serverless and AWS CLI available:

```bash
$  node --version
v14.17.4
$ npm --version
6.14.14
$ serverless --version
Serverless: Running "serverless" installed locally (in service node_modules)
Framework Core: 2.57.0 (local)
Plugin: 5.4.4
SDK: 4.3.0
Components: 3.17.0
$ aws --version
aws-cli/1.19.52 Python/3.8.9 Linux/5.4.122 botocore/1.20.52
```

## Creating the function

I run

```bash
$ serverless create --template aws-nodejs --path my-new-service
```

It created a new folder `my-new-service`.

## Invoking the function locally

I changed to that new folder, and run

```bash
$ serverless invoke local --function hello
Serverless: Running "serverless" installed locally (in service node_modules)
{
    "statusCode": 200,
    "body": "{\n  \"message\": \"Go Serverless v1.0! Your function executed successfully!\",\n  \"input\": \"\"\n}"
}
```

## Deploying the function

First, I configured the AWS credentials:

```bash
serverless config credentials --provider aws --key AKIAXXXX --secret 'YYYY' --region eu-west-1
```

Then, I deployed the function with:

```bash
$ serverless deploy --region eu-west-1
```

## Testing the function

Finally, I was able to invoke the function by running:

```bash
$ serverless invoke --function hello --region eu-west-1
Serverless: Running "serverless" installed locally (in service node_modules)
{
    "statusCode": 200,
    "body": "{\n  \"message\": \"Go Serverless v1.0! Your function executed successfully!\",\n  \"input\": {}\n}"
}
```
