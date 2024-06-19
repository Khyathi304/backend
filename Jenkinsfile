pipeline {
    agent {
                label 'AGENT-1'
    }
     options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
     
    stages {
        stage('Install Dependencies') {
            steps {
                sh """
                npm install
                ls -ltr
                """
            }
        }
    }

     

      post {
        always {
            echo 'I will always say Hello Again!'
            deleteDir()
        }
        success {
            echo 'I will run when the pipeline is success!'
        }
        failure {
            echo 'I will run when the pipeline is failure!'
        }
    }
}




