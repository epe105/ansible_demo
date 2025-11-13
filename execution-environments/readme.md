# Build Execution Environments for Ansible Automation Platform

## Build Docker Image for AWS

ansible-builder build -f aws-execution-environment.yaml -t ee-aws:latest --container-runtime docker --build-arg PLATFORM=linux/amd64

## Push to Docker Hub

docker login
docker tag ee-aws:latest mannyevangelista/ee-aws:latest
docker push mannyevangelista/ee-aws:latest
