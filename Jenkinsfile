pipeline {
  agent any
  environment {
    APPSYSID = '7f8acaa21b352110061b8623604bcbf9'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = 'servicenow'
    DEVENV = 'https://chiarngdevdemo.service-now.com/'
    TESTENV = 'https://chiarngtestdemo.service-now.com/'
    PRODENV = 'https://chiarngproddemo.service-now.com/'
    TESTSUITEID = 'b1ae55eedb541410874fccd8139619fb'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
