// declarative pipe line
pipeline {
    agent { label 'JAVA11' }
    stages {
        stage ('Sourcecode') {
            steps {
                git branch: 'declarative', url: 'https://github.com/Sivarani15/spring-petclinic.git'
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage ( 'Archive and test results') {
            steps {
                junit '**/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/*war', followSymlinks:false
            }
        }
    }
}