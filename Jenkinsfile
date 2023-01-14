pipeline {
    agent any
    tools { 
        maven 'Maven_Home' 
        jdk 'Java_Home' 
    }
stages {
      stage('GIT checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/ijazclouddev/CI-CD-Pipeline-with-Jenkins-deploy-tomcat'
             
          }
        }
        

    
    stage('Compile') {
           steps {
               sh 'mvn clean test package'
            }
        }
        stage('SonarQube Code Analysis') {
        steps{
        withSonarQubeEnv('sonarqube-8.9.10') { 
        sh 'mvn sonar:sonar'
    }
}
}
stage('Build') {
           steps {
               sh 'mvn clean install'
            }
        }
//stage('Upload Build Artifacts'){
//steps{
//nexusArtifactUploader artifacts: [[artifactId: 'hello-world-maven', classifier: '', file: 'target/hello-world-maven.war', type: 'war']], credentialsId: 'Nexus-Credentials', groupId: 'io.happycoding', nexusUrl: '18.181.205.133:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'happycoding-snapshot-repository', version: '1.0-SNAPSHOT'
//}
//} 
/*stage('Upload Build Artifacts'){
    steps{
nexusArtifactUploader artifacts: [[artifactId: 'hello-world-maven', classifier: '', file: 'target/hello-world-maven.war', type: 'war']], credentialsId: 'Nexus-Credentials', groupId: 'io.happycoding', nexusUrl: '18.181.205.133:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-test-repo', version: '1.0-SNAPSHOT'
}
}*/
/*stage('Upload Build Artifacts'){
    steps{
nexusArtifactUploader artifacts: [[artifactId: "${artifactId}", classifier: '', file: 'target/hello-world-maven.war', type: "${packaging}"]], credentialsId: 'Nexus-Credentials', groupId: "${groupId}", nexusUrl: '18.181.205.133:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-test-repo', version: "${version}"
}
}*/
stage('Upload Build Artifacts'){
    steps{
     script{
      def mavenPom = readMavenPom file: 'pom.xml'
nexusArtifactUploader artifacts: [[artifactId: "${mavenPom.artifactId}", classifier: '', file: 'target/hello-world-maven.war', type: "${mavenPom.packaging}"]], credentialsId: 'Nexus-Credentials', groupId: "${mavenPom.groupId}", nexusUrl: '18.181.205.133:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-test-repo', version: "${mavenPom.version}"
}
}
}
stage('Check Ansible version') {
           steps {
               sh 'ansible --version'
            }
        }
//stage('Deploy Application'){
 //   steps{
//sshagent(['deploy_tomcat']) {
    // some block
//sh "scp -o StrictHostKeyChecking=no FirstPipeline/target/hello-world-maven.war ec2-user@13.115.57.250:/tomcat/webapps"
//sh "scp -o StrictHostKeyChecking=no target/hello-world-maven.war ec2-user@13.115.57.250:root/tomcat/webapps"
//sh "scp -o StrictHostKeyChecking=no target/hello-world-maven.war ec2-user@13.115.57.250:/opt/tomcat/webapps"

//}
//}
//}
stage('Execute Ansible Playbook') {
           steps {
             //  withCredentials([string(credentialsId: 'AnsibleVault', variable: 'AnsibleVault'), string(credentialsId: 'vaultdecryptpass', variable: 'vaultdecryptpass')]) {
               sh 'ls -ltrh'
               script{
               def mavenPom = readMavenPom file: 'pom.xml'
               def version = "${mavenPom.version}"  
               echo "${version}"
         //      sh "ansible-playbook -i inventory firstplaybook.yml -e version='${version}'"
               sh "ansible-playbook --vault-password-file=/var/lib/jenkins/workspace/.vault_pass -i inventory roleplaybook.yml -e version='${version}'"
          //      sh 'ansible-playbook --vault-password-file=/var/lib/jenkins/.vault_pass -i inventory mysecondplaybook.yml'
          //     sh "ansible-playbook -i inventory firstplaybook.yml -e version='${version}'"
         //   sh 'ansible-playbook -i inventory firstplaybook.yml'
          //     VAULT_CREDS = credentials("${VAULT_ID}")
           //    FILE = 'secret.txt'
            //   sh "echo '${VAULT_CREDS}' > secret.txt"
         //   sh "echo '${vaultdecryptpass}' >> vaultdecryptpass.txt"
               sh 'ls -ltrh'
          //     sh "ansible-playbook --vault-password-file ./vaultdecryptpass.txt -i inventory firstplaybook.yml -e version='${version}'"
          //     sh 'rm secret.txt'
         // sh 'rm vaultdecryptpass.txt'  
          //     }
               }
            }
        }
}
}
