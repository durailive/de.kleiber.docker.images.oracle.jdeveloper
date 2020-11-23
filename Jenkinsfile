bpipeline {
  agent {
    node {
      label 'localhost'
    }

  }
  stages {
    stage('Build Oracle SQL JDeveloper Image') {
      steps {
        sh 'if [ ! -f $SW_FILE1 ]; then cp "$SW_DIR/$SW_FILE1" $SW_FILE1; fi'
        sh 'if [ ! -f $SW_FILE2 ]; then cp "$SW_DIR/$SW_FILE2" $SW_FILE2; fi'
        sh 'if [ ! -f silent.rsp ]; then cp "silent.rsp" silent.rsp; fi'
        sh 'if [ ! -f create_inventory.sh ]; then cp "create_inventory.sh" create_inventory.sh; fi'
        withCredentials([usernamePassword(credentialsId: 'store.docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh '''docker login --username $USERNAME --password $PASSWORD
sudo docker build --tag oracle/jdeveloper:$SW_VERSION --build-arg SW_FILE1=$SW_FILE1 --build-arg SW_FILE2=$SW_FILE2 .'''
        }
      }
    }
    stage('Push Docker Image to docker hub Registry') {
      steps {
          
       docker push venkateshdevops2020/oracle_jdeveloper_12.2.1.4:latest
            
      }
    }
    stage('Cleanup') {
      steps {
        sh 'docker rmi --force localhost:5000/oracle/jdeveloper:$SW_VERSION'
        sh 'docker rmi --force oracle/jdeveloper:$SW_VERSION'
      }
    }
  }
  environment {
    SW_VERSION = '12.2.1.4'
    SW_FILE1 = 'jdev_suite_122140.jar'
    SW_FILE2 = 'jdev_suite_1221402.jar'
    SW_DIR = '/software/Oracle/JDeveloper'
  }
}
