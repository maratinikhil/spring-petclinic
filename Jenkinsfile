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
        stage ("build & scan") {
            steps {
                withCredentials([string(credentialsId:'Sonarcloud_id', variable: 'SONAR_TOKEN')]){
                withSonarQubeEnv('SONAR'){
                    sh '''mvn package sonar:sonar \
                        -Dsonar.projectKey=maratinikhil_spring-petclinic \
                        -Dsonar.organization=maratinikhil \
                        -Dsonar.host.url=https://sonarcloud.io/ \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
                }
            }
        }
        stage ("Jfrog") {
            steps {
                rtUpload (
                    serverId: 'JFROG',
                    spec: '''{
                        "files": [
                            {
                            "pattern": "target"/*.jar
                            "target": "spc-spc"
                            }
                        ]
                    }''',
                )
            }
            rtPublishBuildInfo(serverId: 'JFROG')    

        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            archiveArtifacts artifacts: 'target/surefire-reports/*.xml'
            junit '**/target/surefire-reports/*.xml'
        }
        success {
            echo "pipeline executed successfully"
        }
        failure {
            echo "pipeline unexpectedly failed" 
        }
    }

}