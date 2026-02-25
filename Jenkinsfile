pipeline {
    agent {label 'SPC'}
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
              withSonarQubeEnv('sonar_id') {
                sh "mvn package sonar:sonar"
              }
            }
        }
    }
}