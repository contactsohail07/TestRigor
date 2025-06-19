pipeline {
    agent any

    environment {
        TESTRIGOR_EMAIL = credentials('contactsohail07@gmail.com') // Jenkins credentials
        TESTRIGOR_API_KEY = credentials('IJQ1IZe7R0w4TBEmFToCklIzEigpO8WatEmN6bq5qPW8htGYoz4p')
        TESTRIGOR_SUITE_ID = 'uXEinE6oADCuroch2'
    }

    stages {
        stage('Install TestRigor CLI') {
            steps {
                sh 'pip install testrigor'
            }
        }

        stage('Run TestRigor Tests') {
            steps {
                sh '''
                testrigor test --email $TESTRIGOR_EMAIL --api_key $TESTRIGOR_API_KEY --suite_id $TESTRIGOR_SUITE_ID
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
        failure {
            echo "Tests failed. Please check the logs."
        }
    }
}
