pipeline {
  agent any
  
  stages {
    stage('Build') {
      steps {
        sh 'docker-compose build'
      }
    }
    
    stage('Test') {
      steps {
        sh 'docker-compose up -d'
        sh 'docker-compose exec web pytest'
      }
    }
    
    stage('Deploy') {
      steps {
        sh 'docker-compose down'
        sh 'docker-compose up -d'
      }
    }
  }
}
