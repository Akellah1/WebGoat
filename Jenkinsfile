pipeline {
    agent any

    tools {
        maven "Maven"
    }

    stages {
        
         stage('SCM') {
             steps {
    git 'https://github.com/WebGoat/WebGoat.git'
  } }
  
        stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
}
        
   //     stage ('Check-Git-Secrets') {
    //       steps {
   //     sh 'rm trufflehog || true'
   //     sh 'docker run gesellix/trufflehog --json https://github.com/WebGoat/WebGoat.git> trufflehog'
   //     sh 'cat trufflehog'
//}
  //  } 
 // stage ('Source Composition Analysis') {
    // steps {
   //     sh 'rm owasp* || true' 
    //  //  sh 'echo "Niksav592_" | sudo -S echo && sudo -S chmod +x /var/lib/jenkins/OWASP-Dependency-Check/owasp-dependency-check.sh '
   //     //sh ' sudo chmod +x /var/lib/jenkins/OWASP-Dependency-Check/owasp-dependency-check.sh | sudo -S command'
        
   //     sh 'bash /var/lib/jenkins/OWASP-Dependency-Check/owasp-dependency-check.sh'
  //     sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
   //     
 //     }
 //   } 
    
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                branches: [[name: '*/master']], 
                extensions: [], 
                userRemoteConfigs: [[url: 'https://github.com/WebGoat/WebGoat.git']]])
            }
        }
        stage('Build') {
            steps {
                git 'https://github.com/WebGoat/WebGoat.git'

               sh "mvn -Dmaven.test.failure.ignore=true clean package"
             //  sh 'mvn clean package'
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
