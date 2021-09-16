pipeline {
  agent any
  triggers {
    pollSCM('H */120 * * 1-5')
  }
  stages {
    stage('Build') {
      steps {
        echo "Build"
        sh 'echo "iMio is here" | grep -i imio'
        sh 'curl -s https://www.imio.be | grep "Restons en contact"'
      }
    }
    stage('Tests'){
      parallel {
        stage('Unit Test') {
          environment {
            KEY = "mysuperkey1234"
          }
          steps {
            sh 'printenv'
          }
        }
        stage('Coverage') {
          steps {
            echo "coverage"
          }
        }
      }
    }
    stage('Deploy') {
      when {
        branch 'production'
      }
      steps {
        echo "Deploy"
      }
    }
  }
  post {
    always {
      echo "My post hello"
      cleanWs()
    }
    failure {
      mail to: "benoit.suttor+testjenkinsci@imio.be",
           subject: "ERROR CI: Project name -> ${env.JOB_NAME}",
           body: "<b>Jenkins CI</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}"
    }

  }
}
