pipeline {
    agent {
                label 'AGENT-1'
    }
     options {
        timeout(time: 30, unit: 'MINUTES') 
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
     environment{
        def appVersion = ''
     }
    stages {
        stage('read the version'){
            steps{
                script{
                def packageJson = readJSON file: 'package.json'
                appVersion = packageJson.version
                echo "application version: $appVersion"
                }
        }
        }
        stage('Install Dependencies') {
            steps {
                sh """
                npm install
                ls -ltr
                echo $appVersion
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




