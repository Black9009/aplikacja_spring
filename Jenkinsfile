pipeline {
    agent{
        label 'dockerslave'
        }
    tools {
        maven "M3"
          }
    environment {
        IMAGE = readMavenPom().getArtifactId()
        VERSION = readMavenPom().getVersion()
              }

    stages {
        stage('Clean application') {
            steps {
                sh 'docker rm -f pandaapp || true' 
                  }
                                    }
        stage('Get code from git') {
            steps {
                //git credentialsId: 'gitpass', url: 'https://github.com/Black9009/aplikacja_spring.git'
                checkout scm
                  }
                                    }
        stage('Build application') {
            steps {
                sh 'mvn clean install'
                  }
                                    }
        stage('Create Docker Image') {
            steps {
                sh'mvn package -Pdocker -Dmaven.test.skip=true'
                  }
                                    }
        stage('Start application') {
            steps {
                sh'docker run -d --name pandaapp -p 0.0.0.0:8080:8080 -t ${IMAGE}:${VERSION}'
                  }
                                    }
        stage('Selenium test') {
            steps {
                sh 'mvn test -Pselenium'
                  }
                                    }
        stage('Deploy to artifactory') {
            steps {
                configFileProvider([configFile(fileId: '85f8e704-929f-425d-a282-04514778519c', variable: 'MAVENSETTINGS')])
                
                {
                    sh 'mvn -s $MAVENSETTINGS deploy '
                    }
                  }
                                    }
        
            }
     
        
    
        
    
}
