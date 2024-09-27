pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        echo 'Corriendo tests...'
        sh 'mvn test'
      }
    }
    stage('SonarQube Analysis') {
      steps {
        echo 'Realizando análisis con SonarQube...'
        withSonarQubeEnv('sonarqube') {
          sh 'mvn sonar:sonar -Dsonar.projectKey=libros -Dsonar.sources=src -Dsonar.java.binaries=target/classes -Dsonar.host.url=http://192.168.64.129:9000/ -Dsonar.login=sqa_291c1a7eea0fc47bfa53591226a1ca0b80da2595'
        }
      }
    }
  }
}
