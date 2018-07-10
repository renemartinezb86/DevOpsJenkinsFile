pipeline {
  agent any
  stages {
    stage('Init') {
      steps {
        echo 'Init'
      }
    }
    stage('Package') {
      steps {
        sh 'zip -r 11-METADATA.zip 11-METADATA'
      }
    }
    stage('BackUp') {
      steps {
        script {
          def remote = [:]
          remote.name = 'test'
          remote.host = '10.49.4.239:22'
          remote.user = 'cbio01'
          remote.password = '*FF89GfQasd'
          remote.allowAnyHosts = true
          stage('Remote SSH') {
            sshCommand remote: remote, command: "cd deploy && mv 11-METADATA.zip 11-METADATA.zip.bak"
          }
        }

      }
    }
    stage('Copy') {
      steps {
        sh 'sshpass -p "*FF89GfQasd" scp 11-METADATA.zip cbio01@10.49.4.239/app01/cbio01/deploy'
      }
    }
    stage('Deploy') {
      steps {
        script {
          def remote = [:]
          remote.name = 'test'
          remote.host = '10.49.4.239:22'
          remote.user = 'cbio01'
          remote.password = '*FF89GfQasd'
          remote.allowAnyHosts = true
          stage('Remote SSH') {
            sshCommand remote: remote, command: "cd /app01/cbio01/appDir/OrderCare18/EOC18/designer/env"
            sshCommand remote: remote, command: "./deploy.sh eoc_v3/eoc_v3@10.49.4.52:1530/EEOCMD03.tdenopcl.internal ENTELCHPOSTDEV_01 1 /app01/cbio01/packages/EOCdeployment true false"
          }
        }

      }
    }
    stage('Restart') {
      steps {
        script {
          def remote = [:]
          remote.name = 'test'
          remote.host = '10.49.4.239:22'
          remote.user = 'cbio01'
          remote.password = '*FF89GfQasd'
          remote.allowAnyHosts = true
          stage('Remote SSH') {
            sshCommand remote: remote, command: "cd /app01/cbio01/appDir/weblogic12C/Oracle/Middleware/Oracle_Home/wlserver/common/bin/WLSTScripts"
            sshCommand remote: remote, command: "sh /app01/cbio01/appDir/weblogic12C/Oracle/Middleware/Oracle_Home/wlserver/common/bin/wlst.sh WLST_RedeployScript.py"
          }
        }

      }
    }
  }
}