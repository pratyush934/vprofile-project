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
        NEXUS_IP='172.31.24.57'
        NEXUS_PORT='8081'
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
             sh "mvn -s settings.xml -DskipTests install"
            }
        }
    }
}