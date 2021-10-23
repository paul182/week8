pipeline {
  environment {
    CALCHOST = "192.168.49.2:31383"
  }
  agent {
    kubernetes {
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'gradle-pod.yaml'
      defaultContainer 'gradle'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    stage("Prepare") {
      steps {
        sh "chmod +x ./gradlew"
        sh "gradle wrapper"
        sh "echo $CALCHOST"
      }
    }
    stage("Acceptance Test") {
      steps {
        sh '''
            ./gradlew acceptanceTest -Dcalculator.url=http://$CALCHOST
            '''
      }
    }
    stage("Test Result") {
      steps {
        publishHTML(target: [
            reportDir: 'build/reports/tests/acceptanceTest',
            reportFiles: 'index.html',
            reportName: "Acceptance test Result"
        ])
      }
    }
  }
}