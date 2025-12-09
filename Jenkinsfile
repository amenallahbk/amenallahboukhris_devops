pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'      // à adapter si tu utilises une autre version dans Jenkins
        maven 'M2_HOME'      // le nom que tu as donné à Maven dans Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')  // ton token SonarQube
    }

    stages {

        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/amenallahbk/amenallahboukhris_devops.git',
                        credentialsId: 'git-credentials'
                    ]]
                ])
            }
        }

        stage('Clean') {
            steps {
                dir('TP-Projet-2025') {   // chemin corrigé
                    sh "mvn clean"
                }
            }
        }

        stage('Compile') {
            steps {
                dir('TP-Projet-2025') {
                    sh "mvn compile"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('TP-Projet-2025') {
                    withSonarQubeEnv('sonarqube') {
                        sh """
                        mvn sonar:sonar \
                          -Dsonar.projectKey=amenallahboukhris_devops \
                          -Dsonar.host.url=http://<IP-ou-URL-de-Sonar>:9000 \
                          -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        stage('Package') {
            steps {
                dir('TP-Projet-2025') {
                    sh "mvn package -DskipTests"
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'TP-Projet-2025/target/*.jar'
        }
    }
}
