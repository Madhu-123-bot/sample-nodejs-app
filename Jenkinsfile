pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Madhu-123-bot/sample-nodejs-app.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("containerguru1/sample-nodejs-app")
        }
      }
    }

    stage('Push Docker Image to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'doc-hub-cred') {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploy to AWS EC2') {
      steps {
        // Set the path to Ansible in case Jenkins can't find it
        withEnv(["PATH+ANSIBLE=/usr/local/bin:/usr/bin"]) {
          ansiblePlaybook(
            playbook: 'playbook.yml',     // Ensure your Ansible playbook is named correctly
            inventory: 'hosts.txt',           // Ensure this file contains the correct EC2 hosts
            credentialsId: 'ansible-ssh-key'
          )
        }
      }
    }
  }
}
