pipeline {
    agent any

    parameters {
         string(name: 'deploy-staging', defaultValue: '192.168.1.10', description: 'Staging Server')
         string(name: 'deploy-prod', defaultValue: '192.168.1.10', description: 'Production Server')
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
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp **/target/*.war ${params.deploy-staging}:/root/apache-tomcat-8.5.83-staging/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war ${params.deploy-prod}:/root/apache-tomcat-8.5.83-staging/webapps/"
                    }
                }
            }
        }
    }
}