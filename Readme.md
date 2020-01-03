Deploying Jenkins

- Install docker
  -- sudo yum update -y
  -- sudo yum install docker
  -- sudo service docker start
  -- sudo usermod -a -G docker ec2-user
  -- log out and in again to apply user permissions

- Install docker compose
  -- sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-\$(uname -m)" -o /usr/local/bin/docker-compose
  -- sudo chmod +x /usr/local/bin/docker-compose
  -- sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

* Install Git
  -- sudo yum install git
  -- git clone https://edofoco@bitbucket.org/edofoco/jenkins-tools.git

-- Install AWS CLI

- Already installed
- aws configure

- Build master
  -- Edit admin and password on Dockerfile
  -- docker build -t jenkins-master .

- Build docker slave
  -- Edit admin and password on Dockerfile
  -- docker build -t jenkins-docker-slave .

- Build dotnet slave
  -- Edit admin and password on Dockerfile
  -- docker build -t jenkins-dotnet-slave .

- Build node slave
  -- Edit admin and password on Dockerfile
  -- docker build -t jenkins-node-slave .

- Run docker-compose.ci.yml
  -- docker-compose -f docker-compose.ci.yml up

- Disable CSRF protection on jenkins
  -- Go to Security Settings and disable CSRF

- Pushing docker
  -- Install ThinBackup plugin
  -- Install CloudBee AWS: Steps plugin
  -- Configure AWS Credentials
  -- Setup restore pipeline using restore repo

-- Install Amazon ECR plugin
Follow this tutorial https://blog.mikesir87.io/2016/04/pushing-to-ecr-using-jenkins-pipeline-plugin/

Source: https://code-maze.com/ci-jenkins-docker/
