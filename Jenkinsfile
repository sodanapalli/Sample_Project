pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage ("scm") {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0c0f4b47-9da9-487d-85c8-85f3a82eb5ef', url: 'https://github.com/sodanapalli/Sample_Project.git']]])
            }
        }
        stage ("sonar analasis") {
            environment {
                scannerHome = tool 'sonarscanner'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage ("quality check"){
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage ('maven buld') {
            steps {
                sh "mvn install"
            }
        }
    }
}
