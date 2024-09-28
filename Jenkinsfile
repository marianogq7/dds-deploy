pipeline {
    agent any
    stages {
        stage('Test con Maven') {
            steps {
                echo 'Corriendo tests...'
                sh 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo 'Ejecutando análisis con SonarQube...'
                withCredentials([usernamePassword(credentialsId: 'sonarqube_credentials', usernameVariable: 'SONAR_USER', passwordVariable: 'SONAR_PASSWORD')]) {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=libros -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=$SONAR_USER -Dsonar.password=$SONAR_PASSWORD -Dsonar.sources=src -Dsonar.token=sqa_291c1a7eea0fc47bfa53591226a1ca0b80da2595' 
                }
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
