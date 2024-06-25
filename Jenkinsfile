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
        nexusUrl = 'nexus.daws304.online:8081'
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
        stage('Build'){
            steps{
                sh """
                zip -q -r backend-${appVersion}.zip * -x backend-${appVersion}.zip
                ls -ltr
                """
            }
        }
        stage('Nexus Artifact Upload'){
            steps{
                script{
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: "backend",
                        credentialsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: "backend",
                            classifier: '',
                            file: "backend-" + "${appVersion}" + '.zip',
                            type: 'zip']
                        ]
                    )
                }
            }
        }

        stage('Deploy'){
            steps{
                script{
                    def params = [
                        string(name: 'appVersion', value: "${appVersion}")
                    ]
                    build job: 'backend-deploy', parameters: params, wait: false
                }
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




