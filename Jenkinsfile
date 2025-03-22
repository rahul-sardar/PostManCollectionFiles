pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                echo("build the war")
            }
        }

        stage("Deploy to QA"){
            steps{
                echo("deploy to qa")
            }
        }

        stage('Checkout') {
            steps {
                git url: 'https://github.com/rahul-sardar/PostManCollectionFiles'
            }
        }
        stage('Run api test cases') {
            steps {
                sh 'newman run ./CRUD_WorkFlow_With_Environment_Variable.postman_collection.json -e ./Basic-Env.postman_environment.json -r cli,htmlextra --reporter-htmlextra-title 		    "CurdWorkflowAPITest" --verbose'
            }
        }
        stage('Publish HTML Extra Report') {
    steps {
        script {
            // Ensure 'results' directory exists
            if (isUnix()) {
                sh 'mkdir -p results'
            } else {
                bat 'mkdir results 2>nul'
            }
        }
        publishHTML([allowMissing: false,
                     alwaysLinkToLastBuild: false, 
                     keepAll: true, 
                     reportDir: 'results', 
                     reportFiles: 'CRUD_report.html', 
                     reportName: 'HTML Extra API Report', 
                     reportTitles: ''])
    }
}

        stage("Deploy to PROD"){
            steps{
                echo("deploy to PROD")
            }
        }
    }
}

