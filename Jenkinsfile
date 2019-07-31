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
        
         stage('Package Stage') {
              steps {
                 sh 'mvn package'
             }    
        }
        
    }
    post {
        always {
           junit 'target/**/*.xml'
            step([
              $class           : 'JacocoPublisher',
              execPattern      : 'target/jacoco.exec',
              sourcePattern    : '**/src/main/java'
           ])
            
        sh "cat ${JENKINS_HOME}/jobs/${env.JOB_NAME}/builds/${env.BUILD_NUMBER}/log > log.txt"
        
        sh "pwd"
        sh "ls -la"

        // sh "curl -F file=@${JENKINS_HOME}/workspace/TestPipeline/log.txt -F channels=builds -H \"Authorization: Bearer xoxp-709486088868-712106001654-711668515429-96a90f6041f25201e6ab38bd33333463\" https://slack.com/api/files.upload"
        
        sh '''
        curl \
            -F token="xoxp-709486088868-712106001654-711668515429-96a90f6041f25201e6ab38bd33333463" \
            -F file=@${JENKINS_HOME}/workspace/TestPipeline/log.txt \
            -F channels="builds" \
            -F as_user=true \
            https://slack.com/api/files.upload
        '''

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
