pipeline {
    agent { 
        node {
            label 'docker-tdp-builder'
            }
      }
    triggers {
        pollSCM '0 1 * * *'
      }
    stages {
        stage('clone') {
            steps {
                echo "Cloning..."
                git branch: '6.0.0-TDP-alliage', url: 'https://github.com/Yanis77240/phoenix-queryserver'
            }
        }
        stage('Build') {
            steps {
                echo "Building..."
                sh '''
                mvn clean install -Dpackage.phoenix.client -Dphoenix.version=5.1.3-TDP-0.1.0-SNAPSHOT -Dphoenix.client.artifactid=phoenix-client-hbase-2.1 -pl '!phoenix-queryserver-it' -DskipTests
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing..."
                sh '''
                mvn clean test -Dpackage.phoenix.client -Dphoenix.version=5.1.3-TDP-0.1.0-SNAPSHOT -Dphoenix.client.artifactid=phoenix-client-hbase-2.1 -pl '!phoenix-queryserver-it' --fail-never
                '''
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                echo "Deploy..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh "mvn clean deploy -Dpackage.phoenix.client -Dphoenix.version=5.1.3-TDP-0.1.0-SNAPSHOT -Dphoenix.client.artifactid=phoenix-client-hbase-2.1 -pl '!phoenix-queryserver-it' -DskipTests -s settings.xml"
                }
            }        
        }
    }
}