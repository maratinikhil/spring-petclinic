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
                sh "mvn package sonar:sonar"
            }
        }
    }
}