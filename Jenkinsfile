// declarative pipe line
pipeline {
    agent {
        label 'MAVEN-3.8.5'
    }
     options { 
        timeout(time: 1, unit: 'HOURS') 
    }
    parameters {
        choice ( name: 'GOAL', choices: [ 'test', 'package', 'cleanpackage'])
    }
    stages {
        stage('Sourcecode') {
            steps {
                git url: 'https://github.com/Sivarani15/spring-petclinic.git' ,
                    branch: 'declarative'
            }
        }
        stage('Buildthecode and sonarqube-analysis') {
            steps {
                withSonarQubeEnv('SONAR_LATEST') {
                    sh script: "mvn ${params.GOAL} sonar:sonar"
                }
                
            }
        }
        stage('Archiving test results') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false

            }
        }
    }
    post {
        success {
            //send success mail
            echo "sucess"
            mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULTS: ${RUN_TESTS_DISPLAY_URL}", cc: '', from: 'devops@qtdevops.com', replyTo: '', 
                subject: " ${JOB_BASE_NAME} BUILD ID: ${BUILD_ID} BUILD NUMBER: ${BUILD_NUMBER} Success", to: 'sivarani42@gmail.com'
            
        }
        unsuccessful {
            //send unsuccess mail
            echo "Unsuccessful"
            mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULTS: ${RUN_TESTS_DISPLAY_URL}", cc: '', from: 'devops@qtdevops.com', replyTo: '', 
                subject: " ${JOB_BASE_NAME} BUILD ID: ${BUILD_ID} BUILD NUMBER: ${BUILD_NUMBER} failed", to: 'sivarani42@gmail.com'
        }
    }
}