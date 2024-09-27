pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        echo 'Corriendo tests...'
        sh 'mvn test' // Asumiendo que usas Maven para los tests
      }
    }
  }
}
