pipeline {
    agent any

    parameters {
         string(name: 'deploy-staging', defaultValue: '3.91.248.239', description: 'Staging Server')
         string(name: 'deploy-prod', defaultValue: '54.226.87.118', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }
   tools {
        maven 'localMaven'
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
                        sh "scp -i /home/ec2-user/id_rsa **/target/*.war ec2-user@${params.deploy-staging}:/home/ec2-user/apache-tomcat-8.5.83/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/ec2-user/id_rsa **/target/*.war ec2-user@${params.deploy-prod}:/home/ec2-user/apache-tomcat-8.5.83/webapps"
                    }
                }
            }
        }
    }
}