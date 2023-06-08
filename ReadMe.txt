Video 1 creating static site image nginx push dockerhub
Dockerfile for niginix

FROM nginx:latest
COPY . /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

Dockerfile for node packagejson 
FROM node:14-alpine as build  
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

build image file in same folder 
sudo docker build -t image_name .

build image file in next folder (Documens=>Project)
sudo docker build -t image_name ./Project

Docker image check 
sudo docker images

sudo docker login

Run Docker container
sudo docker run -p 80:80 image_name

Run docker image in back ground
docker run -d --name web-conatiner -p 80:80 image_name

Check running website
https://localhost/80

Running images check 
sudo docker ps

Running and closed images check 
sudo docker ps -a

Push docker image to docker hub
Create a new docker hub repository

sudo docker tag     Image id       Repository_name
sudo docker tag     71877f47754d   basit56700/test_repo:tagname

sudo docker push basit56700/test_repo:tagname 



Stop docker container 
udo docker stop  conatiner_name/container_id

video 2
create EC2 instance 
search EC2=> instances =>launch instances
 name
 Ubuntu image
 t2.micro
 key.pair=> create new=>name+RSA+.pem

 Allow HTTP traffic from internet (second option)
 launch instance 

 Success => launch instance 

 Run through MobaXtrem
 Session=>SSH=>Public ip address in Remote host =>Ubuntu in Specify user name => Advance setting + add pem file downloaded.=> Bookmarks +Session name add


 Running instance 
 sudo su  //to switch to super user
 apt-get update
 apt-get install docker.io -y   //install docker
 docker --version to check
 //copy repository name from dockerhub
 docker pull repository_name/web:latest
 docker images
 docker run --name web-container -d -p 80:80 image_name
 docker ps   //check docker container running or not

 instance_public_ip_address run on browser to check running image
 http://131.211.121.121

 


YOU CAN WRITE ALL THESE COMMANDS IN USER DATA WHILE CREATING EC2 INSTANCE 



 Troubleshooting
 systemctl status docker
 systemctl start docker
 systemctl enable docker  //docker remain active during restart


 Video 3 deploying S3 buckect using github actions 

create git repository on git hub
upload code on git hub
create a workflow

search S3 on aws

Create bucket 
Bucket name (globally unique) + region + Blocks all public access (allowed)+ create bucket

select bucket from list
 
 properties => static website hosting(disabled=>enabled)=>edit + enable+ default_page_name (mostly index.html)+save changes
 permissions => Bucket policy =>search S3 bucket policy for static site=> copy code=> paste and change bucket name => save

.github folder  
   workflows
      main.yml





      name: build mern image and push to docker hub





on:
  push:
   branches:
      - "master"


jobs:
 
          
  deploy-S3 bucket:
    runs-on:ubuntu-latest

    steps:

   - name: Checkout Repository code
    uses: actions/checkout@v3

    - name: AWS Credential
     uses: aws-actions/configure-aws-credentials@v2
     with:
      aws-access-key-id: ${{secrets.AWS_ACCESS_KEY}}
      aws-secret-access-key: ${{secrets.AWS_SECRET_KEY}}
      aws-region: us-east-1

    name: Push to AWS S3 Bucket
    run: aws s3 sync . s3://aws_bucket_name 