pipeline {
  agent any

  environment {
    CONTAINER_NAME = 'website-builder'
    TARGET_DIR = '/var/www/html'
    DOCKER_IMAGE = 'website' // Define your Docker image name here
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
          docker.image(env.DOCKER_IMAGE).inside {
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
        script {
          sh "docker cp . ${env.CONTAINER_NAME}:/var/www/html"
        }
        echo 'Website published to Apache on port 82'
      }
    }
  }

  post {
    success {
      echo "Pipeline executed successfully for ${env.BRANCH_NAME}"
    }
    failure {
      echo "Pipeline failed for branch ${env.BRANCH_NAME}"
    }
  }
}
