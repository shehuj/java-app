// Jenkinsfile (Declarative Pipeline)

pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'shehuj'
        IMAGE_NAME = "shehuj/shopping_cart-java"
        IMAGE_TAG = "latest"
    }

    tools {
        maven 'maven3'      // Configure this name in "Global Tool Configuration"
        jdk 'jdk11'         // Configure this name as well
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/shehuj/shopping_cart-java.git'
            }
        }

    stage('Setup Tools') {
        steps {
                script {
                  env.JAVA_HOME = tool name: 'jdk11', type: 'jdk'
                  env.PATH = "${env.JAVA_HOME}/bin:${env.PATH}"
                  env.MAVEN_HOME = tool name: 'maven3', type: 'maven'
                  env.PATH = "${env.MAVEN_HOME}/bin:${env.PATH}"
                }
              }
            }
        
        stage('Env Check') {
            steps {
                sh '''
                  echo "JAVA_HOME = $JAVA_HOME"
                  java -version
                  mvn -version
                '''
              }
            }
        
        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
