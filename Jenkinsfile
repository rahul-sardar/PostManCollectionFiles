pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                echo("build the war")
            }
        }

        stage("Deploy to QA") {
            steps {
                echo("deploy to qa")
            }
        }

        stage('Checkout') {
            steps {
                git url: 'https://github.com/rahul-sardar/PostManCollectionFiles'
            }
        }

        stage('Run API Test Cases') {
            steps {
                sh '''
                newman run ./CRUD_WorkFlow_With_Environment_Variable.postman_collection.json \
                -e ./Basic-Env.postman_environment.json -r cli,htmlextra \
                --reporter-htmlextra-title "CurdWorkflowAPITest" --verbose
                '''
            }
        }

        stage('Publish HTML Extra Report') {
            steps {
                script {
                    def latestReport = sh(script: "ls -t newman/*.html | head -1", returnStdout: true).trim()
                    if (latestReport) {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false, 
                            keepAll: true, 
                            reportDir: 'newman', 
                            reportFiles: latestReport,  // Dynamically use latest report
                            reportName: 'HTML Extra API Report', 
                            reportTitles: ''
                        ])
                    } else {
                        error "No report found in newman directory!"
                    }
                }
            }
        }

        stage("Deploy to PROD") {
            steps {
                echo("deploy to PROD")
            }
        }
    }
}
