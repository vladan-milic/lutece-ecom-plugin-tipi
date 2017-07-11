pipeline {

  tools {
    maven 'maven-3.3.9'
  }

  agent any
  
  options { 
    buildDiscarder(logRotator(numToKeepStr: '2'))
    timeout(time: 10, unit: 'MINUTES')
    gitLabConnection('gitlab')
    gitlabCommitStatus(name: 'pending')
  }

  stages {

    stage('Clone repository') {
        steps {
          scm checkout
        }
    }
    
    // BUILD 
    stage('Compile - Tests'){
      steps {
          gitlabCommitStatus {
            sh 'mvn clean install'
          }
        }
    }
   
    // SONAR
    stage('Analyse Sonar'){ 
      when { branch 'develop' }
      steps {
          sh 'mvn sonar:sonar'
      }
    } 

    stage('Analyse Sonar 2'){
      when { expression { BRANCH_NAME ==~ /^feature.*/ } }
      steps {
          echo 'Analyse sonar des commits'        
        }
    }

    // Github
    stage('Update gitHub'){
      when { branch 'master' }
      steps {
        echo 'Mise à jour github'
      }
    }

  }

}