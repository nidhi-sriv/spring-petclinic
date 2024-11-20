pipeline {
    agent none
    stages {
        stage('SCM') {
            agent {
                label "Ubuntu_BuildVM" // NODE_LABEL is an environment variable or parameter
            }
            steps {
                echo "Cloning"
		        git branch: 'main', url: 'https://github.com/nidhi-sriv/spring-petclinic'
		
            }
        }
	stage('BUILD') {
            agent {
                label "Ubuntu_BuildVM" // NODE_LABEL is an environment variable or parameter
            }
            steps {
                echo 'Building the project...'
     		    sh './mvnw package -DskipTests' // Example build command
            }
        }
    stage('CREATEIMAGE') {
            agent {
                label "Ubuntu_BuildVM" // NODE_LABEL is an environment variable or parameter
            }
            steps {
                echo 'Creating Image...'
     		    sh '''
                pwd
                ls -l
                cd target
                docker build -t springpetclinic:v2.0 .
                '''
            }
        }
    stage('PUSHIMAGE') {
            agent {
                label "Ubuntu_BuildVM" // NODE_LABEL is an environment variable or parameter
            }
            steps {
                echo 'Pushing Image...'
     		    sh '''
                docker tag springpetclinic:v2.0 nidhisrivastava20/springpetclinic:v2.0
                docker login
                docker push nidhisrivastava20/springpetclinic:v2.0
                '''
            }
        }  
    stage('SPINUPCONTAINER') {
            agent {
                label "Ubuntu_UT" // NODE_LABEL is an environment variable or parameter
            }
            steps {
                echo 'Pullin Image and spin up container...'
     		    sh '''
                hostname -I
                docker pull nidhisrivastava20/springpetclinic:v2.0
                docker run -d -p 8285:8080 nidhisrivastava20/springpetclinic:v2.0
                '''
            }
        }     
    }
}
