pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    
    stage ('Initialize') {
      steps {
        sh '''
            echo "PATH = ${PATH}"
            echo "M2_HOME = ${M2_HOME}"
           '''
      }
    }
   
    /*stage ('Check-Git-secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/prayag5198/java-goof.git > trufflehog'
        sh 'cat trufflehog'
      }
    }*/
    
   
    
    stage ('SAST') {
      steps {
        //withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
      // sh 'mvn sonar:sonar -Dsonar.projectKey=vulnerable -Dsonar.host.url=http://localhost:9000 -Dsonar.login=3fa37d050a28974d7c366ad918bbdeab997bb040'
        //}
      }
    }
    
    stage ('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    
     /*stage ('Source-Composition-Analysis') {
      steps {
        sh 'rm owasp* || true'
        sh 'wget "https://raw.githubusercontent.com/prayag5198/webapp/master/owasp-dependency-check.sh" '
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'
        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
      }
    }*/
    
    /*stage ('Deploy-To-Tomcat') {
      steps {
        sshagent(['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war anybody@blrl92465.iis.amadeus.net:/prod/apache-tomcat-8.5.50/webapps/webapp.war'
        }
      }
    }*/
    stage('Deploy') {
            steps {
               deploy adapters: [tomcat9(credentialsId: '7d96ddda-581c-45b0-946f-ea885bf11c32', path: '', url: 'http://localhost:8000/')], contextPath: 'java-goof', war: '**/*.war'
            }
        }
  }
}
