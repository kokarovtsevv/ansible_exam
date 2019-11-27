pipeline {

  agent { label 'app' }
  stages {

    stage ( 'Install roles' ) {
      steps {
        sh 'ansible-galaxy install --force -r requirements.yml'
      } 
    }

    stage ( 'Run' ) {
      steps {
        ansiblePlaybook( credentialsId: '4bd0dcd9-8f39-403b-a7c3-645943f21763',
                         inventory: 'inventory/hosts.yml',
                         playbook: 'playbook.yml',
                         vaultCredentialsId: 'vault')   
      }
    }

    stage ( 'Test' ) {
      steps {
        script {
          def response = httpRequest 'http://172.17.0.1:8888'
          println("Response: "+response)
        }
      }
    }
  }
}
