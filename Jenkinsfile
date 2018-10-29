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
		stage('Reemplazando Parametros') {
            steps {
                    script {	
                   echo 'Cambio Parametros'
				   def props = readJSON file: file+'.json'
				   echo props.pipelineConfig.title
				   echo props.pipelineConfig.notifyOnStates
				   echo 'props.pipelineConfig.configuration.constants'
				   props.pipelineConfig.title = 'HolaMundoParametros'
				   props.pipelineConfig.configuration.constants = '{"value" : "amqp://172.22.52.227","key" : "pMQHost"}, {"value" : "V1SERHED","key" : "pMQQueue"}, {"value" : "guest", "key" : "pMQUser" }, { "value" : "guest", "key" : "pMQPassword"}'
				   writeJSON file: file+'.json', json: props, pretty: 4

                }
            }
        }
		stage('Iniciando QA') {
            steps {
                    script {	
                echo 'Iniciando Pipeline ' 
                sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" --data-binary "{"pMQQueue": "test_mq_queue"}" -u "admin:admin" http://127.1.1.0:18630/rest/v1/pipeline/'+pipelineName+'/start?rev=0'
            				}
            }
        }
		
    }
}








