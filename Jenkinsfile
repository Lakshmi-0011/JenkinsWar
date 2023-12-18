pipeline {
    agent any
    environment {
        MAVEN='/opt/apache-maven-3.9.6/bin'
    }

    stages {
        stage('source') {
            steps {
               git branch: 'master', url: 'https://github.com/Lakshmi-0011/JenkinsWar.git'
            }
        }
        stage('build') {
            steps {
                sh "${env.MAVEN}/mvn clean package"
            }
        }
        stage('deploy') {
            steps {
                sshagent(['privatekey']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/*.war ec2-user@34.219.24.208:/opt/tomcat/webapps
                        ssh ec2-user@34.219.24.208 /opt/tomcat/bin/shutdown.sh
                        ssh ec2-user@34.219.24.208 /opt/tomcat/bin/startup.sh
                    """
                }
            }
        }
    }
}
