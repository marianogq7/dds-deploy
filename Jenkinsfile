pipeline {
    agent any
    stages {
        stage('Test con Maven') {
            steps {
                echo 'Corriendo tests...'
                sh 'mvn test'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Construyendo imagen de Docker...'
                sh 'docker build -t alvarogq/libros .'
            }
        }
        stage('Tag y Push a DockerHub') {
            steps {
                echo 'Etiquetando y subiendo la imagen a Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'docker_hub_credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker tag alvarogq/libros alvarogq/libros:latest' // Etiquetando con latest
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD' // Usando las credenciales
                    sh 'docker push alvarogq/libros:latest'
                }
            }
        }
    }
}
