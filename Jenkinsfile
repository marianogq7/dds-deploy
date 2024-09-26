pipeline {
  agent any
  stages {
    stage('Build and Test') {
      steps {
        echo 'Corriendo tests...'
        sh 'mvn test' // Asumiendo que usas Maven para los tests
      }
    }
    stage('Build Docker Image') {
      steps {
        echo 'Armando Imagen de Docker...'
        sh 'docker build -t app-libros -f DockerfileRender .'
      }
    }
    stage('Push Docker Image') {
      steps {
        echo 'Pusheando imagen a registro local..'
        // Aquí puedes hacer push a un registro Docker, si es necesario.
        // Ejemplo para un registro local o Minikube registry:
        sh 'docker tag app-libros 192.168.64.128:30781/app-libros:latest'
        sh 'docker push 192.168.64.128:30781/app-libros:latest'
      }
    }
    stage('Deploy to Minikube') {
      steps {
        echo 'Deploying to Minikube...'
        sh '''
          ssh mgonzalez@192.168.64.128 "kubectl apply -f /home/mgonzalez/tp-vm-prod/deployment.yaml"
        '''
      }
    }
  }
}
