// declarative pipe line
pipeline {
    agent {
        label 'MAVEN-3.8.5'
    }
    tools {
        maven 'maven-3.8.5' 
    }
    options { 
        timeout(time: 1, unit: 'HOURS') 
    }
    parameters {
        choice ( name: 'GOAL', choices: [ 'compile', 'test', 'package', 'clean package'])
    }
    stages {
        stage('Sourcecode') {
            steps {
                git url: 'https://github.com/Sivarani15/spring-petclinic.git' ,
                    branch: 'sprint_develop'
            }
        }
        stage('Artifactory-Configuration') {
            steps {
                rtMavenDeployer (
                    id: 'spc-deployer',
                    serverId: 'JFROG_INSTANCE',
                    releaseRepo: 'qtecomm-libs-release-local',
                    snapshotRepo: 'qtecomm-libs-snapshot-local',

                )
            }
        }
        stage('Build the Code and sonarqube-analysis') {
            steps {

                rtMavenRun (
                    // Tool name from Jenkins configuration.
                    tool: 'MVN_DEFAULT',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    // Maven options.
                    deployerId: 'spc-deployer',
                )

            }
        }
        stage('Archiving test results') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false

            }   
        }
    }
    // post {
    //     success {
    //         //send success mail
    //         echo "sucess"
    //         mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULTS: ${RUN_TESTS_DISPLAY_URL}", cc: '', from: 'devops@qtdevops.com', replyTo: '', 
    //             subject: " ${JOB_BASE_NAME} BUILD ID: ${BUILD_ID} BUILD NUMBER: ${BUILD_NUMBER} Success", to: 'sivarani42@gmail.com'
            
    //     }
    //     unsuccessful {
    //         //send unsuccess mail
    //         echo "Unsuccessful"
    //         mail bcc: '', body: "BUILD URL: ${BUILD_URL} TEST RESULTS: ${RUN_TESTS_DISPLAY_URL}", cc: '', from: 'devops@qtdevops.com', replyTo: '', 
    //             subject: " ${JOB_BASE_NAME} BUILD ID: ${BUILD_ID} BUILD NUMBER: ${BUILD_NUMBER} failed", to: 'sivarani42@gmail.com'
    //     }
    // }
}