pipeline {
    agent {label 'SPC'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage ("git checkout"){
            steps {
                git url: 'https://github.com/maratinikhil/spring-petclinic.git',
                    branch: 'main'
            }
        }
    }
}