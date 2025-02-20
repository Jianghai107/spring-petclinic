pipeline {
    agent any
    
    triggers {
        cron('H/3 * * 4 *')
    }
    
    tools {
        maven 'M3'
        jdk 'JDK17'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Test') {
            steps {
               
                bat 'mvn clean verify'
                
        
                junit '**/target/surefire-reports/*.xml'
            }
        }
        
        stage('Code Coverage') {
            steps {
                jacoco(
                    execPattern: 'target/*.exec',
                    classPattern: 'target/classes',
                    sourcePattern: 'src/main/java',
                    exclusionPattern: 'src/test*'
                )
            }
        }
    }
    
    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'target/site/jacoco',
                reportFiles: 'index.html',
                reportName: 'JaCoCo Coverage Report'
            ])
        }
        
        failure {
            echo 'Pipeline failed! Please check the logs for details.'
        }
    }
}
