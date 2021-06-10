pipeline {
  agent any
  stages {
    stage('Checkout Scm') {
      steps {
        git(credentialsId: '77762aec-11e0-4ff4-91a3-8e9a299ead22', url: 'https://github.com/pathaknitin510/solace-accelerator-demo')
      }
    }

    stage('Shell script 0') {
      steps {
        sh 'ansible-playbook solace-vpn.yml $Tags $Tasks -i $Environment'
      }
    }

  }
  post {
    always {
      echo 'No converter for Publisher: hudson.plugins.emailext.ExtendedEmailPublisher'
      echo 'No converter for Publisher: io.cnaik.GoogleChatNotification'
    }

  }
  parameters {
    choice(name: 'Environment', choices: [Dev-env.ini, Test-env.ini], description: 'This parameter helps in selecting the target Environment.')
    string(name: 'Tags', defaultValue: '--tags', description: 'This parameter helps in executing limited tasks. If you want to run all the tasks, keep this empty.')
    string(name: 'Tasks', defaultValue: 'queues,queue_subscription', description: 'Only above defined tasks will be executed. You can add or remove tasks. If you want to run all the tasks, keep this empty.')
  }
  triggers {
    pollSCM('*/2 * * * *')
  }
}
