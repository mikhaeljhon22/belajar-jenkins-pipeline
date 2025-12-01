pipeline {
    agent none

    environment {
        AUTHOR = "Mikhael"
    }

    parameters {
        string(name: 'NAME', defaultValue: 'Mikhael', description: 'Siapa nama anda?')
        text(name: 'DESC', defaultValue: 'Ini adalah deskripsi', description: 'Masukkan deskripsi anda')
        booleanParam(name: 'FLAG', defaultValue: true, description: 'Apakah anda setuju?')
        choice(name: 'CHOICE', choices: ['Option 1', 'Option 2', 'Option 3'], description: 'Pilih salah satu opsi')
        password(name: 'SECRET', description: 'Masukkan password rahasia')
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }

    stages {

        stage('Parallel Jobs') {
            agent { label "linux && java11" }

            parallel {

                stage("Parameter") {
                    steps {
                        echo "Nama Anda : ${params.NAME}"
                        echo "Deskripsi : ${params.DESC}"
                        echo "Flag : ${params.FLAG}"
                        echo "Pilihan Anda : ${params.CHOICE}"
                        echo "Secret : ${params.SECRET}"
                    }
                }

                stage("Prepare") {
                    steps {
                        echo "Start Jobs : ${env.JOB_NAME}"
                        echo "Start Build : ${env.BUILD_NUMBER}"
                        echo "Start Build : ${env.BUILD_ID}"
                    }
                }

                stage("Prepare Java") {
                    steps {
                        echo "Prepare Java"
                        sleep 5
                    }
                }

                stage("Prepare Maven") {
                    steps {
                        echo "Prepare Maven"
                        sleep 5
                    }
                }

                stage('Build') {
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
                    steps {
                        script {
                            def userInput = input(
                                message: "Apakah anda yakin untuk ke deploy?",
                                ok: "Yes, I'm sure",
                                submitter: "admin, user1",
                                parameters: [
                                    choice(
                                        name: 'TARGET_ENV',
                                        choices: ['Dev', 'QA', 'PROD'],
                                        description: 'Pilih target environment untuk deploy'
                                    )
                                ]
                            )
                            env.TARGET_ENV = userInput
                        }

                        echo "Hello Deploy"
                        sleep 5
                        echo "deploy ke environment ${env.TARGET_ENV}"
                    }
                }

                stage("Release") {
                    when {
                        expression {
                            return params.FLAG
                        }
                    }
                    steps {
                        echo "release aplikasi ke production"
                    }
                }

            } // end parallel
        } // end stage parallel jobs

    } // end stages

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
