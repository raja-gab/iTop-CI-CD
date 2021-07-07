pipeline {
  agent any
  stages {
    stage ('update iTop')
    {
      steps{
        ansibleTowerProjectSync async: false, importTowerLogs: false, project: 'iTop', removeColor: false, throwExceptionWhenFail: false, towerCredentialsId: 'e8eb128e-5be9-45b3-91b3-c8c63031b553', towerServer: 'tower1', verbose: false
          }
    }

    stage('SonarQube Analysis'){
      steps{
        ansibleTower extraVars: '''
        ''', importWorkflowChildLogs: true, jobTemplate: 'iTop-data-extraction', jobType: 'run',  scmBranch: 'develop', throwExceptionWhenFail: false, towerCredentialsId: '3b3ba15f-3642-438e-ab64-53a5ed8baa12', towerLogLevel: 'full', towerServer: 'tower1'
          }
    }
    stage("iTop container build and cleanup deployment") {
      parallel {
          stage ('iTop container build')
          {
            steps{
              ansibleTower extraVars: '''branch: develop
              ''', importWorkflowChildLogs: true, jobTemplate: 'iTop-container-build', jobType: 'run', scmBranch: 'develop', throwExceptionWhenFail: false, towerCredentialsId: '3b3ba15f-3642-438e-ab64-53a5ed8baa12', towerLogLevel: 'full', towerServer: 'tower1'
                }
          }
          stage ('iTop cleanup deployment')
          {
            steps{
              ansibleTower extraVars: '''branch: develop
              ''', importWorkflowChildLogs: true, jobTemplate: 'iTop-cleanup-deployment', jobType: 'run', scmBranch: 'develop',  throwExceptionWhenFail: false, towerCredentialsId: '3b3ba15f-3642-438e-ab64-53a5ed8baa12', towerLogLevel: 'full', towerServer: 'tower1'
                  }
                  }
                  }
    }
    stage ('iTop cleanup container image')
    {
      steps{
        ansibleTower extraVars: 'branch: develop', importWorkflowChildLogs: true, jobTemplate: 'iTop-cleanup-container-image', jobType: 'run', scmBranch: 'develop',  throwExceptionWhenFail: false, towerCredentialsId: '3b3ba15f-3642-438e-ab64-53a5ed8baa12', towerLogLevel: 'full', towerServer: 'tower1'
            }
    }
    stage ('iTop-deploy-k8s')
    {
      steps{
        ansibleTower extraVars: 'branch: develop', importWorkflowChildLogs: true, jobTemplate: 'iTop-deploy-k8s', jobType: 'run', scmBranch: 'develop',  throwExceptionWhenFail: false, towerCredentialsId: '3b3ba15f-3642-438e-ab64-53a5ed8baa12', towerLogLevel: 'full', towerServer: 'tower1'
            }
    }
    stage ('iTop data extraction')
    {
      steps{
        ansibleTower extraVars: '''
        ''', importWorkflowChildLogs: true, jobTemplate: 'iTop-data-extraction', jobType: 'run',  scmBranch: 'develop', throwExceptionWhenFail: false, towerCredentialsId: '3b3ba15f-3642-438e-ab64-53a5ed8baa12', towerLogLevel: 'full', towerServer: 'tower1'
          }
    }
  }
}
