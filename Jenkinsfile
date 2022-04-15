pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Gitcheckout') {
            steps {
              checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'mygit_cred', url: 'https://github.com/caithan/boxfuse-sample-java-war-hello.git']]])
            }
        }
        stage('build') {
            steps {
              bat 'mvn clean install -f boxfuse-sample-java-war-hello/pom.xml'
            }
        }
        stage('codeQuality') {
            steps {
               bat 'mvn sonar:sonar -f boxfuse-sample-java-war-hello/pom.xml'            
            }
        }
        stage('deploy') {
            steps {
            deploy adapters: [tomcat9(credentialsId: 'tomcat_robot', path: '', url: 'http://localhost:7000/')], contextPath: ' boxfuse-sample-java-war-hello', war: '**/*.war'
            }
        }
    }
}
