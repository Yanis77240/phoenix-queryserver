podTemplate(containers: [
    containerTemplate(
        name: 'tdp-builder', 
        image: 'yanisbariteau/tdp-builder:jenkins', 
        command: 'sleep', 
        args: '30d'
        )
  ]) {

    node(POD_LABEL) {
        container('tdp-builder') {
            environment {
                number="${currentBuild.number}"
            }
            stage('Git Clone') {
                echo "Cloning.."
                git branch: '6.0.0-TDP-alliage-k8s', url: 'https://github.com/Yanis77240/phoenix-queryserver'
            }   
            stage ('Build') {
                echo "Building.."
                sh '''
                mvn clean install -Dpackage.phoenix.client -Dphoenix.version=5.1.3-TDP-0.1.0-SNAPSHOT -Dphoenix.client.artifactid=phoenix-client-hbase-2.1 -pl '!phoenix-queryserver-it' -DskipTests
                '''
            }
            /*stage('Test') {
                echo "Testing.."
                sh '''
                mvn clean test -Dpackage.phoenix.client -Dphoenix.version=5.1.3-TDP-0.1.0-SNAPSHOT -Dphoenix.client.artifactid=phoenix-client-hbase-2.1 -pl '!phoenix-queryserver-it' --fail-never
                '''
            }*/
            stage('Deliver') {
                echo "Deploy..."
                withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh "mvn clean deploy -Dpackage.phoenix.client -Dphoenix.version=5.1.3-TDP-0.1.0-SNAPSHOT -Dphoenix.client.artifactid=phoenix-client-hbase-2.1 -pl '!phoenix-queryserver-it' -DskipTests -s settings.xml"
                }
            }
            stage("Publish tar.gz to Nexus") {
                echo "Publish tar.gz..."
                withEnv(["number=${currentBuild.number}"]) {
                    withCredentials([usernamePassword(credentialsId: '4b87bd68-ad4c-11ed-afa1-0242ac120002', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh 'curl -v -u $user:$pass --upload-file phoenix-queryserver-assembly/target/phoenix-queryserver-6.0.0-TDP-0.1.0-SNAPSHOT-bin.tar.gz http://10.110.4.212:8081/repository/maven-tar-files/phoenix_queryserver/phoenix-queryserver-6.0.0-TDP-0.1.0-SNAPSHOT-bin-${number}.tar.gz'
                    }
                }
            }       
        }
    }
}