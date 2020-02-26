pipeline {
  agent any
  options {
      timeout(time: 10, unit: 'MINUTES') 
      retry(3)
  }
  parameters {
      string(name: 'PERSION', 
        defaultValue: 'Mr Huangxuewu', 
        description: 'Who should I say hello to?')      
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
            git(branch: 'master', url: 'https://github.com/xuewu618/PushAPITest')
          }
        }

        stage('CheckUpdate') {
          steps {
            bat 'echo "Hi, %PERSION%, Update project finished."'
            script {
                def browsers = ['chrome', 'firefox']
                for (int i = 0; i < browsers.size(); ++i) {
                    echo "Testing the ${browsers[i]} browser"
                }
                echo "Testing the ${PERSION} browser"
            }
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
            bat 'newman run UserTest.postman_collection.json -r cli,htmlextra --reporter-htmlextra-export .\\newman\\htmlReport-%BUILD_ID%-%BUILD_TIMESTAMP_SIMPLE%.html'
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
        echo 'I will always say Hello again!'
    }
  }
}