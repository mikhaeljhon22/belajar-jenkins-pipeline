pipeline {
    agent {
        node{
            label "linux && java11"
        }
    }
    stages {
        stage('Build') {
            steps {
                echo "Hello Build"
                sleep(5)
                echo "Bangun dari 5 detik"
            }
        }

 stage('Test') {
            steps {
                echo "Hello Test"
                sleep(5)
                echo "Bangun dari 5 detik"
            }
        }

 stage('Deploy') {
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
