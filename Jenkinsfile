pipeline {
  agent any
  stages {
    stage('Test with maven') {
      steps {
        echo 'Corriendo tests...'
        sh 'mvn test'
      }
    }
    stage('Build Docker Image') {
      steps {
        echo 'Construyendo imagen de Docker...'
        sh 'docker build -t alvarogq/libros:latest'
      }
    }
  }
}
