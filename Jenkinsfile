pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '15.206.128.50', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '13.233.96.221', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                        bat ""C:\Program Files (x86)\WinSCP\winscp.exe -i /C/Applications/AWS/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat ""C:\Program Files (x86)\WinSCP\winscp.exe" -i /C/Applications/AWS/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}