def remote = [:]
def git_url = "git@github.com:chesnokov70/flask-poetry"
pipeline {
  agent any
  parameters {
    gitParameter (name: 'revision', type: 'PT_BRANCH')
  }
  environment {
    EC2_USER = "ubuntu"
    USER = "root"
    REGISTRY = "chesnokov70/flask-poetry"
    HOST = '3.92.204.240'
    SSH_KEY = credentials('ssh_instance_key')
    TOKEN = credentials('hub_token')
  }
  stages {
    stage('Configure credentials') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ssh_instance_key', keyFileVariable: 'private_key', usernameVariable: 'username')]) {
          script {
            remote.name = "${env.HOST}"
            remote.host = "${env.HOST}"
            remote.user = "$username"
            remote.identity = readFile("$private_key")
            remote.allowAnyHosts = true
          }
        }
      }
    }
    
    stage ('Clone repo') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: "${revision}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'ssh_github_access_key', url: "$git_url"]]])
      }
    }
    stage ('Build and push') {
      steps {
        script {
         sh """ 
         docker login -u chesnokov70 -p $TOKEN
         docker build -t "${env.REGISTRY}:${env.BUILD_ID}" .
         docker push "${env.REGISTRY}:${env.BUILD_ID}"
         mkdir -p /var/lib/jenkins/.ssh

        ssh-keyscan -H ${HOST} >> ~/.ssh/known_hosts
        chmod 600 ~/.ssh/known_hosts

        scp -i $PRIVATE_KEY -o StrictHostKeyChecking=no $WORKSPACE/docker-compose.tmpl $USER@${HOST}:/opt
        scp -i $PRIVATE_KEY -o StrictHostKeyChecking=no $WORKSPACE/promtail-config.yaml $USER@${HOST}:/opt
        scp -i $PRIVATE_KEY -o StrictHostKeyChecking=no $WORKSPACE/nginx/* $USER@${HOST}:/opt/nginx        
        """
        }
      }
    }
    
    stage ('Deploy flask_poetry') {
      steps {
        script {
          sshCommand remote: remote, command: """
          export APP_IMG="${env.REGISTRY}:${env.BUILD_ID}"
          cd /opt
          envsubst < docker-compose.tmpl | sudo tee docker-compose.yaml         
          docker compose up -d
          """
        }
      }
    }
  }    
} 