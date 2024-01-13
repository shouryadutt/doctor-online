pipeline{
    agent any
    stages{
        stage("Maven Build"){
           steps{
               sh 'mvn package'
           } 
        }
        stage("Artifact Uploader"){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: '</artifactId>   <packaging>w', 
                classifier: '', file: 
               'target/doctor-online', type: 'war']], credentialsId: 'nexus3', 
                groupId: 'in.javahome', nexusUrl: '172.31.41.190:8081', 
                nexusVersion: 'nexus3', protocol: 'http', 
                repository: 'first-release', version: '1.3'
            }
        }
        stage("Dev Deploy"){
           steps{
              sshagent(['tomcat-dev']) {
                // Copy war file to tomcat dev server
                sh "scp -o StrictHostKeyChecking=no target/doctor-online.war ec2-user@172.31.19.43:/opt/tomcat9/webapps/"
                // Restart tomcat server
                sh "ssh ec2-user@172.31.19.43 /opt/tomcat9/bin/shutdown.sh"
                sh "ssh ec2-user@172.31.19.43 /opt/tomcat9/bin/startup.sh"
              }
           } 
        }
    }
}
