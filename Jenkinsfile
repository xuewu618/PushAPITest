pipeline {
  agent any
  options {
      timeout(time: 10, unit: 'MINUTES') 
      retry(3)
  }
  parameters {
      string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

      choice(name:'osType',
        choices:["Android","iOS"],
        description:'测试手机操作系统类型')
  }
  environment { 
      CC = 'clang'
  }
  stages {
    stage(' UpdateProject') {
      parallel {
        stage('PullCode') {
          steps {
            echo "Hello ${params.PERSON}"
            git(branch: 'master', url: 'https://github.com/xuewu618/PushAPITest')
          }
        }

        stage('CheckUpdate') {
          steps {
            bat 'echo Hi, %PERSON%, Update project finished.'
            script {
              def browsers = ['chrome', 'firefox']
              for (int i = 0; i < browsers.size(); ++i) {
                echo "Testing the ${browsers[i]} browser"
              }
              echo "Testing the ${params.PERSON} browser"
            }
            echo "Hello ${params.PERSON}"
            bat 'set'
          }
        }

      }
    }

    stage('RunTest') {
      parallel {
        stage('RunTest') {
          steps {
            echo 'Run test here'
            bat 'newman run UserTest.postman_collection.json -r cli,htmlextra --reporter-htmlextra-export .\\NewmanReports\\index-newmanreport-%BUILD_ID%-%BUILD_TIMESTAMP_SIMPLE%.html'
          }
        }

        stage('CreateReport') {
          steps {
            echo 'Create report here'
          }
        }

        stage('MoveReport') {
          steps {
            echo 'Move report here'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploy here'
      }
    }

    stage('Publish Report') {
	    steps {
	      echo 'Publish Report'
      }
    }

  }
  
  post { 
    always {
      script{
        publishHTML (target: [
          allowMissing: false,
          alwaysLinkToLastBuild: false,
          keepAll: true,
          reportDir: 'NewmanReports',
          reportFiles: 'push-newmanreport-${BUILD_ID}-${BUILD_TIMESTAMP_SIMPLE}.html, user-newmanreport-${BUILD_ID}-${BUILD_TIMESTAMP_SIMPLE}.html',
          reportName: "NewmanHTMLReport"
        ])
      }
    }
  
  	success {
      emailext (
        subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
        body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
            <p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""",
        to: "huangxuewu@huawei.com",
        from: "huangxuewu@huawei.com"
      )
    }
    failure {
      emailext (
        subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
        body: """
            <p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
            <p>Check console output at "<a href="${env.BUILD_URL}/console/">${env.JOB_NAME} [${env.BUILD_NUMBER}] log</a>"</p>
            <p>Check test report at "<a href="${env.BUILD_URL}/NewmanHTMLReport/">${env.JOB_NAME} [${env.BUILD_NUMBER}] report</a>"</p>
            """,
        to: "huangxuewu@huawei.com",
        from: "huangxuewu@huawei.com"
      )
    }
  }
}
