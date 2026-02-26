pipeline {
    agent any
    tools {
        jdk 'jdk'
        maven 'maven'
    }

    environment {
        NEXUS_USER='admin'
        NEXUS_PASS='12345'
        SNAP_REPO='vprofile-snapshot'
        RELEASE_REPO='vprofile-release'
        CENTRAL_REPO='vpro-maven-central'
        NEXUS_GRP_REPO='vprofile-maven-group'
        NEXUSIP='52.66.220.212'
        NEXUSPORT='8081'
        NEXUS_LOGIN='nexus-cred'
    }

    stages {
        stage("Greeintgs") {
          steps {
            echo "Welcome Back!!"
          }
        }
        stage ("Build") {
           
            steps {
                 sh 'mvn -s settings.xml -U -DskipTests install'
            }

            post {
                success {
                    echo "Yes You did it!!!",
                    // archiveArtifacts artifacts: '**/*.war', fingerprint: true
                }
            }

        }

        stage ("Test") {
            step {
                sh "mvn test"
            }
        }

        stage ("CheckStyle Analysis") {
            sh "mvn checkstyle:checkstyle"
        }
    }
}