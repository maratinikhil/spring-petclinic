pipeline {
    agent {label "SPC"}
    stages {
        stage ("git checkout"){
            git url: "https://github.com/maratinikhil/spring-petclinic.git"
                branch: "main"
        }
    }
}