pipeline {
    agent any
     environment {
    def branch = 'master'
    def pipelineName = 'TestMQ80019021-4860-40fa-82bb-296dafb1703e'
	def file= 'Test_MQ'
}
    stages {
        stage('Despliegue QA') {
            steps {
                    script {	
                   echo 'Desplegando Pipeline --> ' +pipelineName
                   sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" --data @'+file+'.json -u "admin:admin" http://127.1.1.0:18630/rest/v1/pipeline/'+pipelineName+'/import?rev=0&overwrite=true&autoGeneratePipelineId=false&includeLibraryDefinitions=true'
                }
            }
        }
		stage('Iniciando QA') {
            steps {
                    script {	
                echo 'cargando codigo fuente desde Api Streamsets ' 
                sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" -u "admin:admin" http://127.1.1.0:18630/rest/v1/pipeline/'+pipelineName+'/start?rev=0' --data-binary '{"pMQQueue": "test_mq_queue"}'
            				}
            }
        }
		
    }
}




