import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import groovy.json.JsonSlurper
import java.net.URL


/* Only keep the 10 most recent builds. */
def projectProperties = [
    [$class: 'BuildDiscarderProperty',strategy: [$class: 'LogRotator', numToKeepStr: '5']],
]
def imageName = 'jenkinsciinfra/jenkinsio'

if (!env.CHANGE_ID) {
    if (env.BRANCH_NAME == null) {
        projectProperties.add(pipelineTriggers([cron('H/30 * * * *'), pollSCM('H/5 * * * *')]))
    }
}

properties(projectProperties)

pipeline {
    agent any
     environment {
    def branch = 'master'
    def pipelineName = 'TestMQ'
	def file= 'Test_MQ'
    def workspace = pwd()
    def pass = ''
    def emptyMap = [:]

}
    stages {
			stage('Reemplazando Parametros') {
            steps {
                    script {	
                   echo 'Cambio Parametros'
				   }
        }
		}
        stage('Parametrizando'){
            steps{
               script{
             Properties prop = new Properties()
             //def content = readFile 'Test_MQ.properties'
             //InputStream is = new ByteArrayInputStream(content.getBytes());
             //properties.load(is)
            InputStream input = null;
           //  try {
            input = new FileInputStream("/var/lib/jenkins/workspace/Test_MQ/Test_MQ.properties");
            prop.load(input);
            //prop.list(System.out);
            echo "Hello World"
            echo "pMQPassword:  ${prop.getProperty("pMQPassword")}"
            pass = prop.getProperty("pMQPassword")
            
             emptyMap = new HashMap(prop);
             emptyMap.each { entry ->
             println "Name: $entry.key Age: $entry.value"
             }
            

            //echo "pMQQueue:  ${properties.pMQQueue}"
            //echo "pMQHost:  ${properties.pMQHost}"
            //echo "pMQUser:  ${properties.pMQUser}"

           // } catch (IOException ex) {
		// ex.printStackTrace();
	// } finally {
		// if (input != null) {
			// try {
			// 	input.close();
			// } catch (IOException e) {
			// 	e.printStackTrace();
               }
              script{
		
	


             def fileJson = workspace+'/'+file+'.json'
             def json = readFile(file:'/var/lib/jenkins/workspace/Test_MQ/Test_MQ.json')
             def data = new JsonSlurperClassic().parseText(json)
             echo " Pass: ${pass}"
              data.pipelineConfig.configuration[12].value[0].value = "amqp://172.22.52.227"
             data.pipelineConfig.configuration[12].value[1].value = "V1SERHED"
             data.pipelineConfig.configuration[12].value[2].value = "guest"
             data.pipelineConfig.configuration[12].value[3].value = "guest"
             //echo " valor:  ${data.pipelineConfig.configuration[12].value[0].value}"
             def json2 = JsonOutput.toJson(data)
             json2 = JsonOutput.prettyPrint(json2)
             writeFile(file:'Test_MQ_wP.json', text: json2)
   
                }
            }
        }
        stage('Despliegue QA') {
            steps {
                    script {	
                   echo 'Desplegando Pipeline --> ' +pipelineName
                   
             def jsonQA = readFile(file:'/var/lib/jenkins/workspace/Test_MQ/Test_MQ_wP.json')
             def data = new JsonSlurperClassic().parseText(jsonQA)
             echo " valor:  ${data.pipelineConfig.configuration[12].value}"

             sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" --data @Test_MQ_wP.json -u "admin:admin" http://172.22.171.20:18630/rest/v1/pipeline/'+pipelineName+'/import?rev=0&overwrite=true&autoGeneratePipelineId=false&includeLibraryDefinitions=true'
			 }
            }
        }
		stage('Iniciando QA PIPELINE_PARAMETERS') {
            steps {
                    script {	
                echo 'Iniciando Pipeline ' 
                sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" -u "admin:admin" http://172.22.171.20:18630/rest/v1/pipeline/'+pipelineName+'/start?rev=0'
               
                   		}
            }
        }
        stage('Iniciando QA RUNTIME_PARAMETERS') {
            steps {
                    script {	
                echo 'Iniciando Pipeline ' 
                
               	sh 'curl -X POST http://172.22.37.20:18630/rest/v1/pipeline/'+pipelineName+'/start?rev=0 --user "rbarriost:Tigo2018" --header "Content-Type:application/json" --header "X-Requested-By:SDC" --header "X-SS-REST-CALL:true"  --data-binary "{"pMQHost": "amqp://172.22.52.227","pMQQueue": "V1CONHED","pMQUser": "guest","pMQPassword":"guest"}"'
                   		}
            }
        }
		
    }
}








