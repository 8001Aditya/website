pipeline {
  agent any

  environment {
    IMAGE_NAME = "8001Aditya/webapp"
    REPO_URL = "https://github.com/8001Aditya/website.git"
    BRANCH_NAME = "${env.GIT_BRANCH}"
  }

  stages {
    stage('Clone') {
      steps {
        git branch: "${env.BRANCH_NAME}", url: "${env.REPO_URL}"
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${IMAGE_NAME}")
        }
      }
    }

    stage('Test') {
      steps {
        script {
          def container = docker.run("${IMAGE_NAME}", "-d -p 80:80")
          sleep(10) // wait for app to boot
          sh 'curl -I localhost || echo "Test Failed"'
          sh 'docker ps -a'
        }
      }
    }

    stage('Deploy to Prod') {
      when {
        branch 'main' // or 'master', depending on fork
      }
      steps {
        script {
          sh 'echo "Deploying to Production..."'
          docker.run("${IMAGE_NAME}", "-d -p 8080:80")
        }
      }
    }
  }
}
