# EMR OUT GOING FAX SERVICE

## THIS SOLTION CONSISTS OF 5 SUB PROJECTS 
    - EMR_Fax_OutGoingService_ExeGenerator
    - EMR_Fax_OutGoing_Service
    - EMR_Fax_OutGoing_UpdateStatus_Service_ExeGenerator
    - EMR_Fax_OutGoing_UpdateStatus_Service
    - BusinessFacade_FaxOutGoing_Service -> common business faccade among all projects

## EMR_Fax_OutGoingService_ExeGenerator
    -> these are main exe's which are deployed on a SERVICES Server and scheduled under a windows task scheduler as a job to run for every 5/10 mins to start the service.

    -> On formLoad these will make a call to DB by using `BF_OutGoingFaxSettingInfo.Get_FaxOutGoingService_Practice_List` and gets all the practices configured and having PENDING Faxes to run this service on the configured DBServer referred from App.config.
        DB USed: `EMR_MasterDB`, SP's used: `usp_Fax_Outoing_Current_Sent_Details_List`    

    -> these will create children exe's/processes for `EMR_Fax_OutGoing_Service` & `EMR_Fax_OutGoing_UpdateStatus_Service` based on the existing running tasks count on CPU must be less than 10.


## EMR_Fax_OutGoing_UpdateStatus_Service_ExeGenerator
