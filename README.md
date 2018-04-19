# UC4-Automic-REST-API-Delivery


_The Repository contains the latest stable WAR file for the Automic Restful API_
 
##Install / Deployment Procedure:##

  1- Deploy the WAR file in your web server (ex: Apache Tomcat)
  
  2- Configure the ConnectionConfig.json file in the root directory of the web app:
  
    
      {"connections":
      	[
        	{
        		"name":"AEPROD",
        		"host":"AETestHost",
        		"dept":"AUTOMIC",
        		"lang":"E",
        		"ports":["2217","2218"],
        		"validity":525600,
        		"RESTadmin":true
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
    * The RESTadmin parameter gives access to certain admin features related to this application, not to AE

  ##How to Get Started:##
  * Once setup, you can use any browser to authenticate with a call similar to this one:
    
    http://hostname:8080/UC4Rest/api/awa/login/v1/Auth?login=BSP&client=200&pwd=MyPassword&connection=AEPROD
  
  => you should get a response with an authentication token:

      {
        "status": "success",
        "token": "gde7oougmpd97kupvde303kunh",
        "expdate": "2017-06-07 11:07:22"
      }

  * Once authenticated, you can use the token to check out the list of objects supported (for ara or awa):

    http://hostname:8080/UC4Rest/api/ara/objects?token=mytoken
    http://hostname:8080/UC4Rest/api/awa/objects?token=mytoken

      {
        "status": "success",
        "count": 9,
        "objects": [
          "Activities",
          "All",
          "Auth",
          "Changes",
          "Client",
          "Engine",
          "Jobs",
          "Misc",
          "Statistics"
        ]
}

  * Once you have the list of objects, you can check out which actions (and which versions) are supported for each object:
     (this can also be done with a POST request in order to display POST actions supported)

    http://hostname:8080/UC4Rest/api/awa/help/v1/All?token=mytoken
    
    {
       "status": "success",
       "type": "GET",
       "count": 2,
       "data": [
         {
           "operation": "export",
           "versions": [
             "exportv1"
           ]
         },
         {
           "operation": "search",
           "versions": [
             "searchv1",
             "searchv2"
           ]
         }
       ]
}

  * Once you have selected an action, you can check out which parameters are required for it, using method=usage as url parameters:

    http://hostname:8080/UC4Rest/api/awa/search/v1/All?method=usage?token=mytoken
    
      {
       "status": "success",
       "required_parameters": [
         "name (format: name= < UC4RegEx > )"
       ],
       "optional_parameters": [
         "search_usage (format: search_usage=Y)"
       ],
       "optional_filters": [],
       "required_methods": [],
       "optional_methods": [
         "usage"
       ],
       "developer_comment": null
     }
    
   * once everything is selected.. you can run operations:
    
    http://192.168.1.60:8080/UC4Rest-0.1/api/awa/search/v1/All?name=CE*&token=mytoken

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
