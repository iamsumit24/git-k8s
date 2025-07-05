pipeline {
  agent any

  environment {
    IMAGE_NAME = "sumittiwari2025/demo-web"
    IMAGE_TAG = "v1"
    DOCKER_CREDENTIALS_ID = "dockerhub-creds"
  }

  stages {
    stage('Clone Code') {
      steps {
        // ✅ Specify correct branch name "main"
        git branch: 'main', url: 'https://github.com/iamsumit24/git-k8s.git'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }

    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh """
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push $IMAGE_NAME:$IMAGE_TAG
          """
        }
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f web-deployment.yaml'
      }
    }
  }
}
