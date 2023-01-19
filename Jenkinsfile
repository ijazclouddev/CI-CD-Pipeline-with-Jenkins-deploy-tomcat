pipeline {
    agent any
    tools { 
        maven 'Maven_Home' 
        jdk 'Java_Home' 
           }
    environment{
      def BRANCH_NAME ='GIT_BRANCH'
  }
stages {
      stage('GIT checkout') {
           steps {
               script{
                   if (env.GIT_BRANCH.contains('main')) {
                echo 'Hello from main branch'
                git branch: 'main', url: 'https://github.com/ijazclouddev/CI-CD-Pipeline-with-Jenkins-deploy-tomcat'
                }
               else {
    //        sh echo 'Hello from ${env.BRANCH_NAME} branch!'"
     //              echo 'Hello from ${env.BRANCH_NAME} branch!
                   echo "Run this stage only if the branch is not main"
                  git branch: 'dev', url: 'https://github.com/ijazclouddev/CI-CD-Pipeline-with-Jenkins-deploy-tomcat' 
               }
               }
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
stage('Upload Build Artifacts'){
    steps{
     script{
         if (env.GIT_BRANCH.contains('main')) {
      def mavenPom = readMavenPom file: 'pom.xml'
nexusArtifactUploader artifacts: [[artifactId: "${mavenPom.artifactId}", classifier: '', file: 'target/hello-world-maven.war', type: "${mavenPom.packaging}"]], credentialsId: 'Nexus-Credentials', groupId: "${mavenPom.groupId}", nexusUrl: '18.181.205.133:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases', version: "${mavenPom.version}"
         }
         else
         {
             echo "Run this stage only if the branch is not main"
             nexusArtifactUploader artifacts: [[artifactId: 'hello-world-maven', classifier: '', file: 'target/hello-world-maven.war', type: 'war']], credentialsId: 'Nexus-Credentials', groupId: 'io.happycoding', nexusUrl: '18.181.205.133:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-test-repo', version: '4.0-SNAPSHOT'
}
}
}
}
stage('Check Ansible version') {
           steps {
               sh 'ansible --version'
            }
        }
stage('Execute Ansible Playbook') {
           steps {
               sh 'ls -ltrh'
               script{
                   if (env.GIT_BRANCH.contains('main')) {
               def mavenPom = readMavenPom file: 'pom.xml'
               def version = "${mavenPom.version}"  
               echo "${version}"
               sh 'ls -ltrh'
               sh "ansible-playbook --vault-password-file=/var/lib/jenkins/workspace/.vault_pass -i inventory roleplaybook.yml -e version='${version}'"
                   }
                   else
                   {
                     echo "Run this stage only if the branch is not main"
                      sh 'ansible-playbook -i devinv devplaybook.yml'
                   }
            }
            }
        }
}
}
