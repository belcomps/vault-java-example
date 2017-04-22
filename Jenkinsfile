pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        withMaven(maven: 'maven-3.2.5') {
          sh 'mvn test'
        }
        
      }
    }
    stage('Compile') {
      steps {
        withMaven(maven: 'maven-3.2.5') {
          sh 'mvn compile'
        }
        
      }
    }
    stage('Package') {
      steps {
        withMaven(maven: 'maven-3.2.5') {
          sh 'mvn package'
        }
        
      }
    }
    stage('Notify') {
      steps {
        echo 'Build Successful!'
      }
    }
    stage('Integration Tests') {
      steps {
        withCredentials([string(credentialsId: 'role', variable: 'ROLE_ID')]) {
        sh 'export SECRET_ID=$(vault write -field=secret_id -f auth/approle/role/java-example/secret-id)'
        sh 'export VAULT_TOKEN=$(vault write -field=token auth/approle/login role_id=${ROLE_ID} secret_id=${SECRET_ID})'
        sh 'export VAULT_ADDR=https://$(hostname):8200'
        sh 'java -jar target/java-client-example-1.0-SNAPSHOT.jar'
        }
      }
    }
  }
  environment {
    mvnHome = 'maven-3.2.5'
  }
}
