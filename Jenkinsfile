pipeline {
  agent any
  stages {
    stage("verify tooling") {
      steps {
        sh '''
          docker version
          docker info
          docker-compose --version 
          curl --version
          
        '''
      }
    }
    stage('Prune Docker data') {
      steps {
        sh 'docker system prune -a --volumes -f'
      }
    }
    stage('Start container') {
      steps {
        sh 'alias kill3000="fuser -k -n tcp 3000"'
        sh 'alias kill80="fuser -k -n tcp 80"'
        sh 'docker rm -f $(docker ps -a -q)'
//         sh 'docker rm -vf $(docker ps -aq)'
        sh 'alias kill3001="fuser -k -n tcp 3001"'
        sh 'docker-compose up --build'
//         sh 'docker compose ps'
      }
    }
    stage('login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('push images') {
      steps {
        sh 'docker push rishabhbhojak/loadapp'
      }
    }
//     stage('Run tests against the container') {
//       steps {
//         sh 'curl http://localhost:3000/param?query=demo | jq'
//       }
//     }
  }
//   post {
//     always {
// //       sh 'docker-compose down'
// //       sh 'docker compose ps'
//     }
//   }
}
