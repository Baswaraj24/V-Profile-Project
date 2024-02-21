pipeline {
   agent any
    
    tools {
        maven 'maven'
        jdk 'jdk11'
    }
    environment  {
       SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Baswaraj24/V-Profile-Project.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-scanner') {
                    sh "mvn sonar:sonar \
                        -Dsonar.projectKey=com.visualpathit:vprofile \
                        -Dsonar.projectName='Visualpathit VProfile' \
                        -Dsonar.sources=src/main"
                }
            }
        }
        stage('Deploy to Tomcat') {
            steps {
           sh "sudo scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline/target/vprofile-v2.war root@13.201.224.146:~/opt/tomcat/webapps/"
          
                }
            }
       }
        post {
        always {
          emailext body: 'The deployment to Tomcat was successful.',
          subject: 'Deployment Status',
          to: 'baswarajkarpe.marolix@gmail.com'
        }
     }
 }
