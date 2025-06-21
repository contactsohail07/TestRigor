pipeline {
    agent any

    environment {
    TESTRIGOR_API_TOKEN = credentials('TESTRIGOR_API_TOKEN')
    TESTRIGOR_SUITE_ID = 'uXEinE6oADCuroch2'
    }

    stages {
        stage('Run TestRigor Tests') {
            steps {
                sh '''
                echo "Starting TestRigor test execution via API"

                curl -X POST \
                  -H 'Content-type: application/json' \
                  -H "auth-token: $TESTRIGOR_API_TOKEN" \
                  --data '{"forceCancelPreviousTesting":true,"storedValues":{"storedValueName1":"Value"}}' \
                  https://api.testrigor.com/api/v1/apps/$TESTRIGOR_SUITE_ID/retest

                sleep 10

                while true
                do
                  echo " "
                  echo "==================================="
                  echo " Checking run status"
                  echo "==================================="
                  response=$(curl -i -o - -s -X GET "https://api.testrigor.com/api/v1/apps/$TESTRIGOR_SUITE_ID/status" -H "auth-token: $TESTRIGOR_API_TOKEN" -H "Accept: application/json")
                  code=$(echo "$response" | grep HTTP |  awk '{print $2}')
                  body=$(echo "$response" | sed -n '/{/,/}/p')
                  echo "Status code: $code"
                  echo "Response: $body"
                  
                  case $code in
                    4*|5*)
                      echo "❌ Error calling API"
                      exit 1
                      ;;
                    200)
                      echo "✅ Test finished successfully"
                      exit 0
                      ;;
                    227|228)
                      echo "⌛ Test is not finished yet"
                      ;;
                    230)
                      echo "❌ Test finished but failed"
                      exit 1
                      ;;
                    *)
                      echo "❓ Unknown status"
                      exit 1
                      ;;
                  esac
                  sleep 10
                done
                '''
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline completed.'
        }
        success {
            echo '🎉 Tests passed successfully.'
        }
        failure {
            echo '🚨 Tests failed. Please review the logs.'
        }
    }
}
