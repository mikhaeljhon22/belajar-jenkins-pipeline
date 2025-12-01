pipeline {
    agent none
    environment{
        AUTHOR="Mikhael"
    }
    stages {

        stage("Prepare") {
            script {
            withCredentials([
                usernamePassword(
                    credentialsId: 'mikhael_rahasia', 
                    usernameVariable: 'APP_USR', 
                    passwordVariable: 'APP_PSW'
                )
            ])
            environment{
                APP = credentials("mikhael_rahasia")
            }
            agent { label "linux && java11" }
            steps {
                echo "Start Jobs : ${env.JOB_NAME}"
                echo "Start Build : ${env.BUILD_NUMBER }"
                echo "Start Build : ${env.BUILD_ID}"
            }
        }

        stage('Build') {
            agent { label "linux && java11" }
            steps {
                echo "Hello Build"
                sleep 5
                echo "Bangun dari 5 detik"
                script {
                    for (int i = 0; i < 5; i++) {
                        echo "perulangan ke ${i}"
                    }
                }
            }
        }

        stage('Test') {
            agent { label "linux && java11" }
            steps {
                echo "Hello Test"
                sleep 5
                echo "Bangun dari 5 detik"
                script {
                    def data = [
                        name: 'Jenkins',
                        type: 'CI/CD'
                    ]
                    writeJSON(file: 'data.json', json: data)
                }
                echo("App user: ${APP_USR}")
                echo("App password: ${APP_PSW}")
                sh("echo 'APP Password : ${APP_PSW}' > secret.txt")
            }
        }

        stage('Deploy') {
            agent { label "linux && java11" }
            steps {
                echo "Hello Deploy"
                sleep 5
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
        failure {
            echo "failure"
        }
        cleanup {
            echo "cleanup"
        }
    }
}
}