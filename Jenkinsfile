// Jenkinsfile (CI‑Only Declarative Pipeline)

pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk11'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/shehuj/shopping_cart-java.git'
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
                  ls -ld $JAVA_HOME/bin/java
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
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'CI Pipeline completed successfully!'
        }
        failure {
            echo 'CI Pipeline failed!'
        }
    }
}
