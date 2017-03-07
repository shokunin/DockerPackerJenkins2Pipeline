node {
  deleteDir()
  checkout scm
  echo 'beginnning workflow...'

  stage 'prepare gems'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  rvm use `cat .ruby-version`@`cat .ruby-gemset`
  gem install bundler
  bundle install --path=.bundle/gems/
  '''

  stage 'Build Container'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  source set_env.sh
  rvm use `cat .ruby-version`@`cat .ruby-gemset`
  bundle exec rake build_full_container
  '''

}
