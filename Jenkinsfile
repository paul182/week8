pipeline {
  agent {
    kubernetes {
      idleMinutes 5  // how long the pod will live after no jobs have run on it
      yamlFile 'gradle-pod.yaml'
      defaultContainer 'gradle'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
  stages {
    stage("prepare") {
      steps {
        sh "chmod +x ./gradlew"
        sh "gradle wrapper"
      }
    }
    stage("Acceptance test") {
      steps {
        sh "./gradlew acceptanceTest -Dcalculator.url=http://$CALCHOST"
      }
    }
    stage("Code coverage") {
      steps {
        sh "./gradlew jacocoTestReport"
        sh "./gradlew jacocoTestCoverageVerification"
      }
    }
  }
}