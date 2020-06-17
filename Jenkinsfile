//comment
pipeline {
 agent any
 stages {
        stage('Checkout-git'){
               steps{
		git poll: true, url: 'git@github.com:falvarezcolq/test-jenkins.git'
               }
        }
        stage('CreateVirtualEnv') {
            steps {
				sh '''
					bash -c "virtualenv env && source env/bin/activate"
				'''

            }
        }
        stage('InstallRequirements') {
            steps {
            	sh '''
            		bash -c "source ${WORKSPACE}/env/bin/activate && ${WORKSPACE}/env/bin/python ${WORKSPACE}/env/bin/pip install -r requirements.txt"
                '''
            }
        }   
        stage('TestApp') {
            steps {
            	sh '''
            		bash -c "source ${WORKSPACE}/env/bin/activate &&  cd src && ${WORKSPACE}/env/bin/python ${WORKSPACE}/env/bin/pytest && cd .."
                '''
            }
        }  
        stage('RunApp') {
            steps {
            	sh '''
            		bash -c "source env/bin/activate ; ${WORKSPACE}/env/bin/python src/main.py &"
                '''
            }
        } 
        stage('BuildDocker') {
            steps {
            	sh '''
            		docker build -t apptest:latest .
                '''
            }
        } 
        stage('PushDockerImage') {
            steps {
            	sh '''
            		docker tag apptest:latest ferwinnerdevelop/apptest:latest
					docker push ferwinnerdevelop/apptest:latest
					docker rmi apptest:latest
                '''
            }
        } 
  }
}


