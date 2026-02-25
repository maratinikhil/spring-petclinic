pipeline {
    agent {label: 'SPC'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage ('git checkout') {
            steps {
                git url: 'https://github.com/maratinikhil/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('build & scan') {
            steps {
                withCredentials([string(credentialsId: 'My_Sonarqube_id', variable: 'SONAR_TOKEN')])
                withSonarQubeEnv ('SONAR')
                sh """
                    mvn package sonar:sonar
                    -Dsonar.projectKey=maratinikhil_spring-petclinic \
                    -Dsonar.organization=maratinikhil \
                    -Dsonar.host.url=https://sonarcloud.io \
                    -Dsonar.login=$SONAR_TOKEN
                """
            }
        }
    }
}