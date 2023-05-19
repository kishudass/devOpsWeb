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
    
    environment {
        fname = "Kishore"    
        lname = "Dass"
        version = "1.2"
        systems = "Dev"
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
                        echo "This is made by ${env.fname} ${env.lname}"
                        echo "It is running on ${env.systems} and the version is ${env.version}"
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
