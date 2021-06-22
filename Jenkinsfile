pipeline {
   agent any

   stages {
      
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      
      stage('Docker Build') {
         steps {
            pwsh(script: 'docker images -a')
            pwsh(script: """
               cd azure-vote/
               docker images -a
               docker build -t ljupchokostic/jenkins:pipeline .
               docker images -a
               cd ..
            """)
         }
      }  
      
      stage('Start test app') {
         steps {
            pwsh(script: """
               docker-compose up -d
               ./scripts/test_container.ps1
            """)
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Run Tests') {
         steps {
            pwsh(script: """
               #python3 -m unittest ./tests/test_sample.py
               python3 --version
            """)
         }
      }
      stage('Stop test app') {
         steps {
            pwsh(script: """
               docker-compose down
            """)
         }
      }
      
   }
}
