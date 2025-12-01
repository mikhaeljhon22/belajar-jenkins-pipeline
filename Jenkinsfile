pipeline {
    agent none 

    environment {
        AUTHOR = "Mikhael"
    }

    // triggers {
    //     cron('1 * * * *')
    //     //PollSCM('1 * * * *')
    //     //upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS)
    // }

    parameters {
        string(name: 'NAME', defaultValue: 'Mikhael', description: 'Siapa nama anda?')
        text(name: 'DESC', defaultValue: 'Ini adalah deskripsi', description: 'Masukkan deskripsi anda')
        booleanParam(name: 'FLAG', defaultValue: true, description: 'Apakah anda setuju?')
        choice(name: 'CHOICE', choices: ['Option 1', 'Option 2', 'Option 3'], description: 'Pilih salah satu opsi')
        password(name: 'SECRET', defaultValue: 'secret', description: 'Masukkan password rahasia')
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }

    stages {

        stage("Parameter") {
            agent {
                node {
                    label "linux && java11"
                }
            }
            steps {
                echo "Nama Anda : ${params.NAME}"
                echo "Deskripsi : ${params.DESC}"
                echo "Flag : ${params.FLAG}"
                echo "Pilihan Anda : ${params.CHOICE}"
                echo "Secret : ${params.SECRET}"
            }
        }

        stage("Prepare") {
            agent { label "linux && java11" }
            steps {
                echo "Start Jobs : ${env.JOB_NAME}"
                echo "Start Build : ${env.BUILD_NUMBER}"
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

                withCredentials([
                    usernamePassword(
                        credentialsId: 'mikhael_rahasia',
                        usernameVariable: 'APP_USR',
                        passwordVariable: 'APP_PSW'
                    )
                ]) {
                    echo("App user: ${APP_USR}")
                    echo("App password: ${APP_PSW}")
                    sh("echo 'APP Password : ${APP_PSW}' > secret.txt")
                }
            }
        }

        stage('Deploy') {
            input{
                message "Apakah anda yakin untuk ke deploy?"
                ok "Yes, I'm sure"
                submitter "admin, user1"
                parameters{
                    choice(name: 'TARGET_ENV', choices: ['Dev','QA','PROD'], description: 'Pilih target environment untuk deploy?')
                }
            }
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
