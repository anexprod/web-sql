pipeline {
  agent any 

  environment {
  DOCKER_CREDENTIALS_ID = 'docker-credentials-id'
  DOCKER_IMAGE = "slash/web-sql"

  }

  stages {
        stage ('Clone repository') {
          steps {
          git branch: 'main' , url: 'https://github.com/anexprod/web-sql.git'
    }
        }
        stage ('Build Docker image') {
          steps {
            script {
              docker.build(DOCKER_IMAGE)
    }
          }
        }
        stage ('Run tests'){
          steps {
            sh "echo 'run some tests'"
          }
        }
      }
        stage ('Push to Docker Hub'){
          when {
            expression {currentBuild.currentResult == 'SUCCESS'
                       }
            steps {
              script {
                docker.withRegistry('https://index.docker.io/v1', DOCKER_CREDENTIALS_ID){
                  docker.image(DOCKER_IMAGE).push
                }
              }
            }
      }
        post {
          failer {
            echo "Build or test failed"
        }
      }
  }
} 
