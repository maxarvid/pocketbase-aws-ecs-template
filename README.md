# Pocketbase on AWS ECS Fargate
This repo serves as an example for deploying Pocketbase to AWS ECS Fargate. There are some caveats, but for small personal projects it has so far been adequate. 

## The why?
I enjoy Pocketbase. I enjoy deploying Docker containers. I enjoy the promise of AWS Fargate Spot pricing.

## The caveats
- At the time of writing, Pocketbase is still in active development
- AWS Fargate tasks are ephemeral. This stack mounts a volume using AWS EFS for the SQLite persistence layer, which is NOT recommended and probably not the right tool for the job. But AWS EBS at the time of writing isn't the right tool either when it comes to Fargate.
- The stack spins up its own VPC, might be overkill.
- The stack spins up an Application Loadbalancer, might be overkill.
- If you deploy with `aws cloudformation deploy`, you will need to build the Docker image, login to your ECR, grab the URI of the ECR repo, push the built image to ECR, and then trigger a redeploy of the ECS service. Good times.
- To get around the NFS nature of AWS EFS, we set `--logMaxDays=0` flag.
- If you want HTTPS, you will need to attach a cert to your Application Loadbalancer.

## TODOs
- Adds some scripting to deal with some of the more annoying parts above.
