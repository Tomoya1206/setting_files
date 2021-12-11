import hudson.model.ParameterValue
import hudson.model.StringParameterValue
import java.lang.Object

def selectURL(){
    if(BUILD_TYPE == 'Release'){
        return 'Release'
    }
    else if(BUILD_TYPE == 'Debug'){
        return 'Debug'
    }
}

List<ParameterValue> commonParams =[
    new StringParameterValue('BUILD_TYPE' ,"${BUILD_TYPE}"),
    new StringParameterValue('INTEG' ,"${INTEG}"),    
]


/* def common_param = [
    'name:'+"BUILD_TYPE"+' ,value:'+"${BUILD_TYPE}",
    'name:'+"INTEG"+',value:'+"${INTEG}",
] */

def common_param_name = [
    "BUILD_TYPE" ,
    "INTEG" ,
]

def common_param_value = [
    "${BUILD_TYPE}",
    "${INTEG}",
]

pipeline{
    agent any

    parameters{
        choice(name:'BUILD_TYPE',choices:'Release\nDebug',description:'')
        choice(name:'INTEG',choices:'Yes\nNo',description:'')
    }


    stages{
        stage('test'){
            steps{
                script{
                    for(def i = 0 ; i <  common_param_name.size(); i++){
                    echo 'name:'+"${common_param_name[i]}"+',value:'+"${common_param_value[i]}" 
                    }
/*                     common_param.each{String val ->
                    echo "${val}"
                    } */ 
                }
            }
        }

        stage('call_job'){
                steps{
                    build(
                        job: "called_job",
                        parameters:
                            //  script{
                                /*for(def i = 0 ; i <  1; i++){
                                string(name:"${common_param_name[i]}",value:"${common_param_value[i]}")
                                }*/
/*                             common_param.each{val ->
                            string("${val}")
                            }    */
                            commonParams,                            
                            //}  
                            //string(name:"${common_param_name[0]}",value:"${common_param_value[0]}"),
                            //string(name:"${common_param_name[1]}",value:"${common_param_value[1]}"),
                            //string(name:"BUILD_TYPE" ,value:"${BUILD_TYPE}"),
                            //string(name:"INTEG" ,value:"${INTEG}"),
                            
                            )
                    }  
                }
    }
}