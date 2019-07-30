pipeline {
    agent any
    stages {
        stage('Compile Stage') {
             steps {
                 sh 'mvn clean compile'
             }      
        }
        
        stage('Test Stage') {
             steps {
                 sh 'mvn test'
             }      
        }
        
         stage('Clean Install Stage') {
             steps {
                 sh 'mvn clean install'
             }      
        }
        
        stage('Publish test results') {
            steps {
               junit '**/test-results/test/*.xml'
            }
        }
        
         stage('Package Stage') {
              steps {
                 sh 'mvn package'
             }    
        }
    
    }
    post {
        always {
          slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        success {
             echo 'I am Success'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
          slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
