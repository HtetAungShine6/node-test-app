// pipeline {
//     agent any
//     stages {
//         stage('Build') { 
//             steps {
//                 sh 'npm install' 
//             }
//         }
//     }
// }


pipeline {
  agent any

  tools {
    nodejs 'node20'
  }

  stages {
    stage('Install') { steps { sh 'npm install' } }
    stage('Test')    { steps { sh 'npm test' } }
    stage('Build')   { steps { sh 'npm run build' } }
  }
}