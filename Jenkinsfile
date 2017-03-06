node {
  deleteDir()
  checkout scm
  echo 'beginnning workflow...'

  stage 'prepare gems'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  bundle install --path=.bundle/gems/
  '''

  stage 'Check Environment'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  bundle exec rake check-env
  '''

}
