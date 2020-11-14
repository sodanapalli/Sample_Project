pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage ("scm") {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0c0f4b47-9da9-487d-85c8-85f3a82eb5ef', url: 'https://github.com/sodanapalli/Sample_Project.git']]])
            }}
        }
        stage ("sonar analasis") {
            environment {
                scannerHome = tool 'sonar'
            }
            steps {
                withSonarQubeEnv('sonar') {
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
        stage ('artifact uploder') {
            steps {
                nexusPublisher nexusInstanceId: 'nexus3', nexusRepositoryId: 'My-release', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/pipelline/target/simple-web-app.war']], mavenCoordinate: [artifactId: 'simple-web-app', groupId: 'org.mitre', packaging: 'war', version: '2.65']]]
            }
        }
    }
}
