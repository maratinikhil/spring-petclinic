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
                    withCredentials([string(credentialsId: 'sonar_id', variable: 'SONAR_TOKEN')]){
                    withSonarQubeEnv('SONAR'){
                        sh """mvn package sonar:sonar \
                            -Dsonar.projectKey=maratinikhil_spring-petclinic \
                            -Dsonar.organization=maratinikhil \
                            -Dsonar.host.url=https://sonarcloud.io/ \
                            -Dsonar.login=$SONAR_TOKEN
                            """
                    }
                    }
                } 
            }
            stage ('Artifactory - Jfrog'){
                steps {
                    rtUpload (
                        serverId: 'Jfrog',
                        spec: '''{
                            "files": [
                                {
                                "pattern": "target/*.jar",
                                "target": "spcjava-spc"
                                }
                            ]
                        }''',
                    )
                     rtPublishBuildInfo(serverId: 'Jfrog')    
                }

            }
        }
post {
    always {
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
         archiveArtifacts artifacts: 'target/surefire-reports/*.xml'
        junit '**/target/surefire-reports/*.xml'
    }
    success {
        echo 'pipeline executed successfully!'
    }
    failure {
        echo 'pipeline failed. check the logs for details'
    }
}
}
