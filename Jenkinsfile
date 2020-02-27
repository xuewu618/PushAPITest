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
            bat 'newman run UserTest.postman_collection.json -r cli,htmlextra --reporter-htmlextra-export .\\NewmanReports\\index.html'
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

  }
  post { 
    always { 
      script{
        publishHTML (target: [
          allowMissing: false,
          alwaysLinkToLastBuild: false,
          keepAll: true,
          reportDir: 'NewmanReports',
          reportFiles: 'index.html',
          reportName: "NewmanHTMLReport"
        ])
      }
    }
  }
}