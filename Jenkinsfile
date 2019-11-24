pipeline {

  agent { label 'app' }
  stages {

    stage ( 'Install roles' ) {
      steps {
        sh 'ansible-galaxy install -r requirements.yml'
      } 
    }

    stage ( 'Run' ) {
      steps {
        ansiblePlaybook( credentialsId: 'jenkins',
                         installation: 'ansible',
                         inventory: 'inventory/hosts.yml',
                         playbook: 'playbook.yml',
                         vaultCredentialsId: 'vault')   
      }
    }

    stage ( 'Test' ) {
      steps {
        script {
          def response = httpRequest(url: 'http://172.17.0.4:8888', validResponseCodes: '200')
          println( response )
        }
      }
    }
  }
}
