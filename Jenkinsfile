pipeline {
    agent any
    stages {
        stage('Build docker') {
            steps {
                sh 'docker-compose up --build -d'
            }
        }
        stage('Test') {
            steps {
                sh 'docker exec recipe_back_mspr mvn -B -f /home/app/pom.xml test'
            }
        }
        stage('Build') {
            steps {
                sh 'docker exec recipe_back_mspr mvn -B -f /home/app/pom.xml -DskipTests package'
            }
        }
        stage('SonarQube') {
            steps {
                sh 'docker exec recipe_back_mspr mvn -B -f /home/app/pom.xml sonar:sonar'
            }
        }
    }
    post {
        always {
            emailext to: "paul.aboulinc@gmail.com",
                     subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
                     attachLog: true,
                     body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            sh 'docker-compose down'
        }
    }
}
