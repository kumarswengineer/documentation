# EMR OUT GOING FAX SERVICE

## THIS SOLTION CONSISTS OF 5 SUB PROJECTS 
    - EMR_Fax_OutGoingService_ExeGenerator
    - EMR_Fax_OutGoing_Service
    - EMR_Fax_OutGoing_UpdateStatus_Service_ExeGenerator
    - EMR_Fax_OutGoing_UpdateStatus_Service
    - BusinessFacade_FaxOutGoing_Service -> common business faccade among all projects

## EMR_Fax_OutGoingService_ExeGenerator (Windows Forms Type Project)
    
    -> This is a main exe which is deployed on a SERVICES Server and scheduled under 
       a windows task scheduler as a job to run for every 5/10(confirm with n/w team once) mins to start the service.

    -> On formLoad(frmFAX_OutGoingServiceStarter_Load) it will read the DBServers configured to run on this server from 
       App.config(key: DBServerInfo - add server names with ; delimiter) and will loop on each DBServer and makes 
       a call to this method `Run_EMR_FaxOutgoingService_For_AllPractices`.
       
    -> In this we will make a call to DB by using `BF_OutGoingFaxSettingInfo.Get_FaxOutGoingService_Practice_List` 
       and gets all the practices configured and having PENDING Faxes to run this service on the configured DBServer
       referred from App.config(key: DBServerInfo - add server names with ; delimiter).
       
       DB USed: `EMR_MasterDB`, SP's used: `usp_Fax_Outoing_Current_Sent_Details_List`
    
    -> the SP will return the list of PracticeID, PK_Job (all FAX job ids delimited by , coming from SP) which are PENDING  to send fax.

    -> Now Loop on resulting list of PENDING practices list and create children exe's/processes for `EMR_Fax_OutGoing_Service` 
       for each practice using `Start_OutGoingFax_Using_MetroFax_Process()` based on the existing running tasks count on CPU 
       must be less than 10 with `GetExistingProcessCount()`.
    
    -> `Start_OutGoingFax_Using_MetroFax_Process` this method will create a children exe by passing command line args as follows
        PracticeID & "^#" & PK_Jobs & "^#"


## EMR_Fax_OutGoing_UpdateStatus_Service_ExeGenerator
