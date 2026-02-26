pipeline {
    agent {label 'SPC'} 
        stages {
            stage ('git checkout') {
                steps {
                    git url: 'https://github.com/maratinikhil/spring-petclinic.git',
                        branch: 'main'
                }
            }
            stage ('build & scan') {
                steps {
                    withCredentails([string(CredentialsId: 'SONAR', variable: 'SONAR_TOKEN')]){
                    withSonarQubeEnv('SONAR'){
                        sh """mvn package sonar:sonar \
                              -Dsonar.projectkey=maratinikhil_spring-petclinic \
                              -Dsonar.organization=maratinikhil \
                              -Dsonar.host.url=https://sonarcloud.io \
                              -Ddonar.login=$SONAR_TOKEN 
                        """
                    }
                    }
                } 
            }
        }
}