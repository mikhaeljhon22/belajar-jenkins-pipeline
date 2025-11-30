pipeline {
    agent none

    stages{
        stage("Prepare"){
            agent{
                node{
                    label "linux & java11"
                }
            }
           steps{
             echo("Start Jobs : ${env.JOB_NAME}")
             echo("Start Build : ${env.BUILD_NUMBER }")
             echo("Start Build : ${env.BUILD_ID}")
           }
        }
    }
    stages {
        stage('Build') {
              agent {
        node{
            label "linux && java11"
        }
    }
            steps {
                echo "Hello Build"
                sleep(5)
                echo "Bangun dari 5 detik"
              script{
                for(int i=0; i<5; i++){
                    echo "perulangan ke ${i}"
                }
              }
            }
        }

 stage('Test') {
      agent {
        node{
            label "linux && java11"
        }
    }
            steps {
                echo "Hello Test"
                sleep(5)
                echo "Bangun dari 5 detik"
            script{
                def data = [
                    name: 'Jenkins',
                    type: 'CI/CD'
                ]
                writeJSON(file: 'data.json', json:data)
            }
            }
        }

 stage('Deploy') {
      agent {
        node{
            label "linux && java11"
        }
    }
            steps {
                echo "Hello Deploy"
                sleep(5)
                echo "Bangun dari 5 detik"
            }
        }
    }
    post {
        always {
            echo "This will always run"
        }
        success {
            echo "This will success run"
        }
        failure{
            echo "failure"
        }
        cleanup{
            echo "cleanup"
        }
    }
}
