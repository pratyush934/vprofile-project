pipeline {
    agent any

    tools {
        jdk 'jdk'
        maven 'maven'
    }

    environment {
        NEXUS_USER      = 'admin'
        NEXUS_PASS      = '12345'
        SNAP_REPO       = 'vprofile-snapshot'
        RELEASE_REPO    = 'vprofile-release'
        CENTRAL_REPO    = 'vpro-maven-central'
        NEXUS_GRP_REPO  = 'vprofile-maven-group'
        NEXUSIP         = '52.66.220.212'
        NEXUSPORT       = '8081'
        NEXUS_LOGIN     = 'nexus-cred'
    }

    stages {

        stage("Greetings") {
            steps {
                echo "Welcome Back!!"
            }
        }

        stage("Build") {
            steps {
                sh 'mvn -s settings.xml -U -DskipTests clean install'
            }

            post {
                success {
                    echo "Yes You did it!!!"
                    // archiveArtifacts artifacts: '**/*.war', fingerprint: true
                }
            }
        }

        stage("Test") {
            steps {
                sh 'mvn test'
            }
        }

        stage("CheckStyle Analysis") {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
    }
}