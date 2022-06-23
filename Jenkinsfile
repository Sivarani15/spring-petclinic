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
        // stage('Build') {
        //     steps {
        //         sh 'mvn clean package'
        //     }
        // }
        stage('Artifactory configuration') {
            steps {
                rtMavenDeployer (
                    id: 'spc-deployer',
                    serverId: 'JFROG_INSTANCE',
                    releaseRepo: 'qtecomm-libs-release-local',
                    snapshotRepo: 'qtecomm-libs-snapshot-local'
                )
            }
        }

        stage('Artifactory deploy') {
            steps {
                rtMavenRun (
                    // Tool name from Jenkins configuration.
                    tool: MVN_DEFAULT,
                    // Set to true if you'd like the build to use the Maven Wrapper.
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'spc-deployer'
                )        
            }
        }
        stage('reporting') {
            steps {
                junit testResults: 'target/surefire-reports/*.xml'
            }
        }
    }
}