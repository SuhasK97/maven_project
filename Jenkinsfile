pipeline {
    agent { label 'git_Pipeline' }

    options {
        timestamps()
    }

    environment {
        GIT_REPO   = 'git@github.com:SuhasK97/maven_project.git'
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
                    credentialsId: 'git_pipeline'
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
