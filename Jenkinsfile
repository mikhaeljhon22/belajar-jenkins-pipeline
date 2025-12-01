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

        /* =============================================================
                         MATRIX OS SETUP
        ============================================================= */
        stage("OS setup") {
            matrix {
                axes {
                    axis {
                        name 'OS'
                        values 'linux', 'windows', 'macos'
                    }
                    axis {
                        name 'ARCH'
                        values '32', '64'
                    }
                }

                excludes {
                    exclude {
                        axis {
                            name 'OS'
                            values 'macos'
                        }
                        axis {
                            name 'ARCH'
                            values '32'
                        }
                    }
                }

                stages {
                    stage("OS Setup") {
                        agent { label "linux && java11" }
                        steps {
                            echo "Setup ${OS} dengan arsitektur ${ARCH}"
                        }
                    }
                }
            }
        }

        /* =============================================================
                         PARAMETER PRINT
        ============================================================= */
        stage("Parameter") {
            agent { label "linux && java11" }
            steps {
                echo "Nama Anda : ${params.NAME}"
                echo "Deskripsi : ${params.DESC}"
                echo "Flag : ${params.FLAG}"
                echo "Pilihan Anda : ${params.CHOICE}"
                echo "Secret : ${params.SECRET}"
            }
        }

        /* =============================================================
                         PREPARE STAGES
        ============================================================= */
        stage("Prepare") {
            agent { label "linux && java11" }
            steps {
                echo "Start Jobs : ${env.JOB_NAME}"
                echo "Start Build : ${env.BUILD_NUMBER}"
                echo "Start Build ID : ${env.BUILD_ID}"
            }
        }

        stage("Prepare Java") {
            agent { label "linux && java11" }
            steps {
                echo "Prepare Java"
                sleep 5
            }
        }

        stage("Prepare Maven") {
            agent { label "linux && java11" }
            steps {
                echo "Prepare Maven"
                sleep 5
            }
        }

        /* =============================================================
                         BUILD
        ============================================================= */
        stage('Build') {
            agent { label "linux && java11" }
            steps {
                echo "Hello Build"
                sleep 5
                echo "Bangun dari 5 detik"

                script {
                    for (int i = 0; i < 5; i++) {
                        echo "Perulangan ke ${i}"
                    }
                }
            }
        }

        /* =============================================================
                         TEST
        ============================================================= */
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

                    // Simpan file tanpa bocorkan password ke log
                    sh """
                        echo "APP_USER=${APP_USR}" > secret.txt
                        echo "APP_PASSWORD=${APP_PSW}" >> secret.txt
                    """
                }
            }
        }

        /* =============================================================
                         DEPLOY (WITH MANUAL INPUT)
        ============================================================= */
        stage('Deploy') {
            agent { label "linux && java11" }
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
                echo "Deploy ke environment ${env.TARGET_ENV}"
            }
        }

        /* =============================================================
                         RELEASE
        ============================================================= */
        stage("Release") {
            agent { label "linux && java11" }
            when {
                expression { return params.FLAG }
            }
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'mikhael_rahasia',
                        usernameVariable: 'APP_USR',
                        passwordVariable: 'APP_PSW'
                    )
                ]) {
                    sh """
                        echo "Release User: ${APP_USR}" > release-info.txt
                        echo "Release Password: ${APP_PSW}" >> release-info.txt
                    """
                }
            }
        }
    }

    /* =============================================================
                         POST ACTIONS
    ============================================================= */
    post {
        always { echo "This will always run" }
        success { echo "Pipeline Success!" }
        failure { echo "Pipeline Failed!" }
        cleanup { echo "Cleanup Done" }
    }
}
