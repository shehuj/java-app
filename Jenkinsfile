pipeline {
    agent any
    tools{
        jdk  'jdk11'
        maven  'maven3'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, credentialsId: '15fb69c3-3460-4d51-bd07-2b0545fa5151', poll: false, url: 'https://github.com/jaiswaladi246/Shopping-Cart.git'
            }
        }
        
        stage('COMPILE') {
            steps {
                sh "mvn clean compile -DskipTests=true"
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
        
        stage('Docker Build & Push') {
            steps {
                script{
                    withDockerRegistry(credentialsId: '2fe19d8a-3d12-4b82-ba20-9d22e6bf1672', toolName: 'docker') {
                        
                        sh "docker build -t shopping-cart -f docker/Dockerfile ."
                        sh "docker tag  shopping-cart adijaiswal/shopping-cart:latest"
                        sh "docker push adijaiswal/shopping-cart:latest"
                    }
                }
            }
        }
        
        
    }
}
