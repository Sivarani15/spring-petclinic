pipeline {
    agent {
        label 'JAVA11'
    }
    stages {
        stage('Sourcecode') {
            steps {
                git url:'https://github.com/Sivarani15/spring-petclinic.git', branch:'sprint_develop'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}