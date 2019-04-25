Deploying Jenkins

- Install docker
-- sudo yum update -y
-- sudo yum install docker
-- sudo service docker start
-- sudo usermod -a -G docker ec2-user
-- log out and in again to apply user permissions

- Install docker compose
-- sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
-- sudo chmod +x /usr/local/bin/docker-compose
-- sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose


- Install Git
-- yum install git
-- git clone https://edofoco@bitbucket.org/edofoco/jenkins-tools.git

-- Install AWS CLI
- Already installed
- aws configure

- Build master
-- Edit admin and password on Dockerfile
-- docker build -t jenkins-master .

- Build slave
-- Edit admin and password on Dockerfile
-- docker build -t jenkins-slave .

- Run docker-compose.ci.yml
-- docker-compose -f docker-compose.ci.yml up

- Pushing docker
-- Install CloudBee AWS Crendentials plugin
-- Install Amazon ECR plugin
Follow this tutorial https://blog.mikesir87.io/2016/04/pushing-to-ecr-using-jenkins-pipeline-plugin/

Source: https://code-maze.com/ci-jenkins-docker/



