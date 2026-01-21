pipeline {
    agent { label 'linuxgit' }

    options {
        timestamps()
    }

    environment {
        GIT_REPO   = 'https://gitlab.com/sandeep160/pipeline-e2e.git'
        BRANCH     = 'main'

        MAVEN_HOME = tool 'Maven3'
        JAVA_HOME  = tool 'openjdk21'
        PATH       = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {

        // -------------------- CHECKOUT --------------------
        stage('Checkout Repo') {
            steps {
                git branch: BRANCH,
                    url: GIT_REPO,
                    credentialsId: '246e4390-6513-40d3-9379-a13be88e6658'
            }
        }

        // -------------------- BUILD --------------------
        stage('Build') {
            steps {
                sh '''
                    cd maven
                    mvn clean compile
                '''
            }
        }

        // -------------------- LINT (CHECKSTYLE) --------------------
        stage('Lint') {
            steps {
                sh '''
                    cd maven
                    mvn checkstyle:check
                '''
            }
        }

        // -------------------- UNIT TESTS --------------------
        stage('Unit Tests') {
            steps {
                sh '''
                    cd maven
                    mvn test
                '''
            }
            post {
                always {
                    junit allowEmptyResults: true,
                          testResults: 'maven/target/surefire-reports/*.xml'
                }
            }
        }
    }
}
