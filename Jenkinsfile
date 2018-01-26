pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: '127.0.0.1:8080', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '127.0.0.1:9090', description: 'Production Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh "cp **/target/*.war /opt/apache-tomcat-8.5.27-staging/webapps"
                    }
                }

                stage('Deploy to Production') {
                    steps {
                        sh "cp **/target/*.war /opt/apache-tomcat-8.5.27-prod/webapps"
                    }
                }
            }
        }
    }
}
