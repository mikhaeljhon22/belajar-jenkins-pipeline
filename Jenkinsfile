pipeline {
    agent {
        node{
            label "linux && java11"
        }
    }
    stages {
        stage('Hello') {
            steps {
                echo "Hello Pipeline"
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
