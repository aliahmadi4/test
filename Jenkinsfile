pipeline {
    agent any
//     agent {
//         docker { image 'public.ecr.aws/docker/library/gradle:7.6.1-jdk17' }
//     }
//     tools {
//         maven 'Maven-3.8.4'
//         gradle 'Gradle-7.6.1'
//     }
    // this section configures Jenkins options
    options {

        // only keep 10 logs for no more than 10 days
        buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '10'))

        // cause the build to time out if it runs for more than 12 hours
        timeout(time: 12, unit: 'HOURS')

        // add timestamps to the log
        timestamps()
    }

    // this section configures triggers
    triggers {
          // create a cron trigger that will run the job every day at midnight
          // note that the time is based on the time zone used by the server
          // where Jenkins is running, not the user's time zone
          cron '@midnight'
    }

    // the pipeline section we all know and love: stages! :D
    stages {
        stage('Source') {
            steps {
                echo 'Source...'
//                 sh 'mvn --version'
                sh 'git --version'
                git branch: 'master',
                    url: 'https://github.com/aliahmadi4/test.git'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
//                 sh 'mvn clean'
                sh './gradlew test'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                sh './gradlew build'
            }
        }
        stage('Dockerize...') {
            steps {
                echo 'Dockerizing....'
                sh './gradlew docker'
            }
        }
        stage('Report') {
            steps {
                echo 'Reporting....'
            }
        }
    }

    // the post section is a special collection of stages
    // that are run after all other stages have completed
    post {

        // the always stage will always be run
        always {

            // the always stage can contain build steps like other stages
            // a "steps{...}" section is not needed.
            echo "This step will run after all other steps have finished.  It will always run, even in the status of the build is not SUCCESS"
        }
    }
}
