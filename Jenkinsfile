pipeline {
  agent any

  environment {
    CONTAINER_NAME = 'website-builder'
    TARGET_DIR = '/var/www/html'
  }

  stages {
    stage('Clone Repository') {
      steps {
        git url: 'https://github.com/aanyaraj/website2.git', branch: "${env.BRANCH_NAME}"
      }
    }

    stage('Build Inside Docker') {
      steps {
        script {
          docker.image(DOCKER_IMAGE).inside {
            sh 'cp -r * /var/www/html'
          }
        }
      }
    }

    stage('Publish (if master)') {
      when {
        branch 'master'
      }
      steps {
        sh 'docker cp . website:latest:/var/www/html'
        echo 'website published to Apache on port 82'
      }
    }
  }

  post {
    success {
      echo "pipeline executed successfully for ${env.BRANCH_NAME}"
    }
    failure {
      echo "Pipeline failed for branch ${env.BRANCH_NAME}"
    }
  }
}
