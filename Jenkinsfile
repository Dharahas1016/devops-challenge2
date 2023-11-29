pipeline {

  environment {
    frontendDockerImageName = "dharahas1016/sample-frontend:v1"
    backendDockerImageName = "dharahas1016/sample-backend:v1"
    dockerImage = ""
    registryCredential = 'dockerhublogin'
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Dharahas1016/devops-challenge2.git'
      }
    }

    stage('Build Frontend Image') {
      steps {
        script {
          dockerImage = docker.build frontendDockerImageName
        }
      }
    }

    stage('Push Frontend Image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("${frontendDockerImageName}")
          }
        }
      }
    }

    stage('Build Backend Image') {
      steps {
        script {
          dockerImage = docker.build backendDockerImageName
        }
      }
    }

    stage('Push Backend Image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("${backendDockerImageName}")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "k8s-deployment-service.yaml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
