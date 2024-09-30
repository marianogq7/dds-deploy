pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                echo 'Corriendo tests con Maven...'
                sh 'mvn test'
            }
        }
        stage('Analisis') {
            steps {
                echo 'Ejecutando análisis con SonarQube...'
                withCredentials([usernamePassword(credentialsId: 'sonarqube_credentials', usernameVariable: 'SONAR_USER', passwordVariable: 'SONAR_PASSWORD')]) {
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=libros \
                            -Dsonar.sources=src \
                            -Dsonar.java.binaries=target/classes \
                            -Dsonar.host.url=http://sonarqube:9000 \
                            -Dsonar.login=$SONAR_USER \
                            -Dsonar.password=$SONAR_PASSWORD \
                            -Dsonar.exclusions=src/test/**
                    '''
                }
            }
        }
        stage('Build Image') {
            steps {
                echo 'Construyendo imagen de Docker...'
                sh 'docker build -t alvarogq/libros .'
            }
        }
        stage('Tag & Push') {
            steps {
                echo 'Etiquetando y subiendo la imagen a Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'docker_hub_credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker tag alvarogq/libros alvarogq/libros:latest' // Etiquetando con latest
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD' // Usando las credenciales
                    sh 'docker push alvarogq/libros:latest'
                }
            }
        }
        stage('Deployment') { 
            steps {
                echo 'Desplegando aplicación en Minikube...'
                sh 'ssh mgonzalez@192.168.64.129 "kubectl apply -f /home/mgonzalez/deploy-app/"'
            }
        }
    }
}
