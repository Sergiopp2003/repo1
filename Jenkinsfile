pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonarqube-token')
        NEXUS_CREDENTIALS = credentials('nexus-credentials')
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git url: 'https://github.com/Sergiopp2003/repo1.git
', branch: 'main'
            }
        }

        stage('Compilar') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Analizar con SonarQube') {
            environment {
                SONAR_HOST_URL = 'http://localhost:9000'
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Publicar en Nexus') {
            steps {
                sh """
                    mvn deploy -DaltDeploymentRepository=nexus::default::http://localhost:8081/repository/maven-releases/ \
                        -Dnexus.username=${NEXUS_CREDENTIALS_USR} \
                        -Dnexus.password=${NEXUS_CREDENTIALS_PSW}
                """
            }
        }
    }
}
