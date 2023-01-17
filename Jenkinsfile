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
                   sh echo 'Hello from ${env.GIT_BRANCH} branch!'
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
stage('Execute Ansible Playbook') {
           steps {
               sh 'ls -ltrh'
               script{
               def mavenPom = readMavenPom file: 'pom.xml'
               def version = "${mavenPom.version}"  
               echo "${version}"
               sh 'ls -ltrh'
               sh "ansible-playbook --vault-password-file=/var/lib/jenkins/workspace/.vault_pass -i inventory roleplaybook.yml -e version='${version}'"
            }
            }
        }
}
}
