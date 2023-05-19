pipeline {
    agent any
    
    tools {
        maven 'local_maven'
    }
    parameters {
         string(name: 'tomcat_stag', defaultValue: '54.198.128.17', description: 'Node1-Remote Staging Server1')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp **/*.war root@${params.tomcat_stag}:/opt/tomcat/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        echo 'This is just a demo on Production server.'
                    }
                }
            }
        }
    }
}
