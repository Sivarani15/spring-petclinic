// declarative pipe line
pipeline {
    agent {
        label 'JAVA11'
    }
     options { 
        timeout(time: 1, unit: 'HOURS') 
    }
    triggers {
        cron('0 * * * *')
    }
    stages {
        stage ('Sourcecode'){
            steps {
                git url: 'https://github.com/Sivarani15/spring-petclinic.git' ,
                    branch: 'main'
            }
        }
        stage ('Buildthecode') {
            steps {
                sh script: 'mvn clean package'
            }
        }
        stage ('Archiving test results') {
            junit testResults: 'target/surefire-reports/*.xml'
            archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
        }
    }
    post {
        success {
            //send success mail
            echo "sucess"
        }
        unsuccessful {
            //send unsuccess mail
            echo "Unsuccessful"
        }
    }
}