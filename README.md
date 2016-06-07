# UC4-Automic-REST-API-Delivery

_The Repository contains the latest stable WAR file for the Automic Restful API_
 
##Install / Deployment Procedure:##

  1- Deploy the WAR file in your web server (ex: Apache Tomcat)
  
  2- Configure the ConnectionConfig.json file in the root directory of the web app:
  
    
      {"connections":
      	[
        	{
        		"name":"AEPROD",
        		"host":"192.168.1.60",
        		"dept":"AUTOMIC",
        		"lang":"E",
        		"ports":["2217","2218"],
        		"validity":525600
        	}
      	]
      }

  * what you need to do is add en entry (or update the existing one) with your own parameters:
    * You can declare as many connections as you want (their "name" needs to be unique thought)
    * You can modify the validity period (expressed in minutes)
    * The host can be an IP adress or a hostname
  
  * what you need to know about this file:
    * This config file is used to shield users from having to know Department, hostname and ports of the AE.. also to control the validity period
    * The "name" is what needs to be provided when authenticating via the REST call (url parameter: name=)
    * The validity is expressed as a number of minutes (525600 minutes = 1 year in the example above)

  ##how to get started:##
    * Once setup, you can use any browser to authenticate with a call similar to this one:
        
    http://192.168.1.60:8080/UC4Rest-0.1/api/awa/login/v1/Auth?login=BSP&client=200&pwd=Un1ver$e&connection=AEPROD
  
  => you should get a response with an authentication token:

      {
        "status": "success",
        "token": "gde7oougmpd97kupvde303kunh",
        "expdate": "2017-06-07 11:07:22"
      }
      
    * Then you can use the token to run other commands:
    
    http://192.168.1.60:8080/UC4Rest-0.1/api/awa/search/v1/Jobs?name=CE*&token=k2lcl4vofh1g5m8f14v159tbrk
    
  => you should get a nice JSON response from it:
  
      {
        "success": true,
        "count": 2,
        "data": [
          {
          "name": "CE.JOBS.WEBSERVICE.REST",
          "folder": "0200/POCS/DELIVERED/CON.EDISON/PART4.BONUS.PART.WEB.SERVICES",
          "title": "REST Job for ConEd",
          "type": "JOBS",
          "open": ""
          },
          {
          "name": "CE.JOBS.WIN.1",
          "folder": "0200/POCS/DELIVERED/CON.EDISON/PART1.INTRODUCTION",
          "title": "ConEd Windows Cmd without Parameter",
          "type": "JOBS",
          "open": ""
          }
        ]
      }
