pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/Madhu-123-bot/sample-nodejs-app.git'
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
        ansiblePlaybook(
          playbook: 'playbook.yml',     // Ensure your Ansible playbook is named correctly
          inventory: 'hosts',           // Ensure this file contains the correct EC2 hosts
          credentialsId: 'ansible-ssh-key'
        )
      }
    }
  }
}
