pipeline {
    agent {
        label 'james'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'babylon', url: 'git@github.com:jjoyneriv/examples.git']]])
            }
        }    
        stage('Clean') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
		sh 'mvn compile'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Static Analysis for Sonar') {
            steps {
                withSonarQubeEnv('YSBSonarqube') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }
        stage('Sonar Gate') {
            steps {
                sh 'echo "sonar gate ok"'
            } 
        }
        stage('Build & Publish') {
            steps {
                sh 'mvn package'
		sh 'mvn install'
                sh 'echo "publish to artifactory ok"'
                // hygieiaArtifactPublishStep artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2"
            } 
        }
        stage('Deploy to DEV') {
            steps {
                sh 'echo "deploy to dev ok"'
            }
            post { 
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Success', environmentName: 'DEV'
                }
                failure {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Failure', environmentName: 'DEV'
                }
            }
        }
        stage('Deploy to QA') {
            steps {
                sh 'echo "deploy to qa ok"'
            }
            post { 
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Success', environmentName: 'QA'
                }
                failure { 
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Failure', environmentName: 'QA'
                }
            }
        }
        stage('Deploy to Stage') {
            steps {
                sh 'echo "deploy to stage ok"'
            }
            post { 
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Success', environmentName: 'STAGE'
                }
                failure {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Failure', environmentName: 'STAGE'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                sh 'echo "deploy to production ok"'
            }
            post { 
                success {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Success', environmentName: 'PROD'
                }
                failure {
                    // hygieiaDeployPublishStep applicationName: 'my-jacksonvilleapp2', artifactDirectory: 'target', artifactGroup: 'com.jacksonville.app', artifactName: '*.jar', artifactVersion: "1.2-SNAPSHOT-JKVE2", buildStatus: 'Failure', environmentName: 'PROD'
                }
            }
        }
    }
}
