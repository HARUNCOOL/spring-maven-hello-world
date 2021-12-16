pipeline {
    agent any
    
     tools {
        // Install the Maven version configured as "maven 3.8" and add it to the path.
        maven "maven 3.8.1"
    }

    stages {
        stage('SCM') {
            steps {
                // Get some code from a GitHub repository
               git branch: 'master', credentialsId: 'git-credentials', url: 'https://github.com/markondareddy/maven-web-application.git'
               }

        }
     stage('Clean') {
             steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean"
                }
        }
        
         stage('Compile') {
             steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true compile"
                }
        }
  
        stage('Package') {
             steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true package"
				
                }
        }
        
         stage('Artifact') {
            steps {
                // maven artifacts
               archiveArtifacts 'target/*.war'
              // archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
        
        stage('Connect tomcat-linux Server') {
            steps {
               withCredentials([sshUserPrivateKey(credentialsId: 'tomcat-linux-credentials', keyFileVariable: 'key-variable', passphraseVariable: 'variable', usernameVariable: 'linux-user')]) {
                 
                 echo 'Successfully connected tomcat linux isntance.................'
                  }
            }
        }
        
        stage('Tomcat Server') {
            steps {
             withCredentials([usernameColonPassword(credentialsId: 'tomcat-testing', variable: 'markonda_tomcat')]) { 
                  sh "curl -v -u ${markonda_tomcat} -T /var/lib/jenkins/workspace/pipeline-repo/target/maven-web-application.war 'http://ec2-54-175-30-179.compute-1.amazonaws.com:8080/manager/text/deploy?path=/lokesh-app&update=true'"
              }
            }
        }
    

    }
}