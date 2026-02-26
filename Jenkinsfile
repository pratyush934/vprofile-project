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
        SCANNER_HOME=tool 'sonar-scanner'
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

        stage("CheckStyle Analysis") {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage("SonarQube Analysis") {
            environment {
                SCANNER_HOME = tool 'sonar-scanner'
            }
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=vprofile \
                        -Dsonar.projectKey=vprofile \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.junit.reportsPath=target/surefire-reports \
                        -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                        -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                    '''
                }
            }
        }

        stage("Quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token' 
                }
            } 
        }
         stage("UploadArtifact"){
            steps{
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                  groupId: 'QA',
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                  repository: "${RELEASE_REPO}",
                  credentialsId: "${NEXUS_LOGIN}",
                  artifacts: [
                    [artifactId: 'vproapp',
                     classifier: '',
                     file: 'target/vprofile-v2.war',
                     type: 'war']
                  ]
                )
            }
        }

    }
}