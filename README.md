# Benchmarks

**Updated: February 23, 2022**

Tests performance of various proxies/load balancers. Based on the blog post https://www.loggly.com/blog/benchmarking-5-popular-load-balancers-nginx-haproxy-envoy-traefik-and-alb/. This is a fork of https://github.com/NickMRamirez/Proxy-Benchmarks

We test the following proxies in their latest versions as of 23th of February 2022:

* Caddy 2.4.6
* Envoy 1.21.1
* HAProxy 2.6
* NGINX 1.21.6
* Traefik 2.6.1

**IMPORTANT! Be sure to SSH into the client VM and run the test against the proxy_server VM from there. Running the test from within the AWS VPC will reduce Internet latency.**

Defaults to the AWS "eu-central-1" region.

NOTE: The AWS plugin for Terraform can be finicky. The deployment may or may not work the first time. In that case, 
use `terraform taint aws_instance.proxy_server` for example to try it again.

## Setup

Perform these steps:

1. In the [AWS Console](https://console.aws.amazon.com), create a new SSH keypair (default name is "benchmarks"):
    * Go to __EC2 > Key Pairs > Create Key Pair__.
    * Name it "benchmarks".
    * Save the .pem file to this project's directory.
    * Update the file's permissions with `chmod 400 ./benchmarks.pem`
2. Run:
```
terraform init
terraform apply -auto-approve -var 'aws_access_key=<YOUR_ACCESS_KEY>' -var 'aws_secret_key=<YOUR_SECRET_KEY>'
```
3. Log into the *client* server with `ssh -i ./benchmarks.pem ubuntu@<IP_ADDRESS>` and run the tests:

```
/tmp/run_tests.sh | tee results.txt
```

Performance tests use https://github.com/rakyll/hey.

To tear down the servers:

```
terraform destroy -force -var 'aws_access_key=<YOUR_ACCESS_KEY>' -var 'aws_secret_key=<YOUR_SECRET_KEY>'
```
