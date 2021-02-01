All of these files are built on the openApi spec (3.0). You can go to https://editor.swagger.io/ and import these files (separately). This will give you a ‘pretty print’ of the endpoints and operations, and there are options to generate a server and/or client from there, which is fairly handy for quicker testing. 

All data is transferred in JSON format.
All IDs are in UUID format.
All timestamps comply to ISO 8601 date time formats.

There are three APIs described in these files, representing different parts or paradigms of the workflow of the interaction between FRAIHMWORK and your service.


***USS API FOR HEALTH AND INTEGRITY MONITORING***
This is a set of APIs that can be implemented *by the service* that FRAIHMWORK can pull information down from. This is useful for systems that already plan to host their own service information for other services to consume. It also negates the need for the service to reach out and ping the FRAIHMWORK system with updated information at regular intervals.


**FRAIHMWORK HEALTH AND INTEGRITY MONITORING SERVER API***
This is very similar to the previous API, but assumes that the client server is pushing its data to FRAIHMWORK at regular intervals (or on changes). In this paradigm, the service uses GET, POST, PUT, and DELETE HTTP calls to FRAIHMWORK to inform it of changes in system details, state, and general health.


***REGISTRATION API***
Since we have no formal way of keeping track of components within the system of systems, FRAIHMWORK attempts to do so by providing a Registration Service, which just provides unique UUIDs to components that want to be tracked by FRAIHMWORK. It is capable of creating new UUIDs for any purpose, logging simple service information and associating it with a provided UUID, and can access existing registrations if information needs to be found on other components. 


***DATA TYPES***

SYSTEM - This is the basic type that represents a single system, service or component. Systems are associated with a UUID once they enter the FRAIHMWORK system, and this UUID can be assigned by the service itself, requested from the Registration API and then provided, or it can be generated under the hood by the FRAIHMWORK server. The data type provides a plethora of different data fields (only a few of which are required) to track things like its version, current state, name, model, manufacturer, etc. It also provides flexibility for providing any additional details that the system can provide, which may or may not be used by the rest of the FRAIHMWORK system. Implementors are encouraged to use as many of the data fields as possible.

FAULT - This is the type that represents something wrong going on with the system. Faults have a name, description, code and severity at the most basic level, which provide all of the important information about what it is and how it is affecting the service reporting it. The data type also provides details about when the fault was first discovered, and when it was last determined to be there (timeOfDetection, and timeOfValidity, respectively). Faults are associated and identified with a UUID.



***WORK FLOW FOR SYSTEMS HOSTING SERVER API***

Registration workflow:
S <---request system info --- F
S ---Generates UUID or requests from F if it hasn't done so --->
S ---provides system info --> F


Fault / System Reporting
S <--Requests Fault / System info--- F
S ----Returns requested info ------> F

***WORK FLOW FOR SYSTEMS USING CLIENT API*** 
Assume that S is a service and F is the FRAIHMWORK system.

Registration workflow A:
S ----request uuid----> F
S <-----new uuid ------ F
S -post system w/ id -> F
S <---Registered ID---- F

Registration workflow B:
S <--generates own UUID-->

S -post system w/ id -> F
                        F <--internally registers S -->
S <---Registered ID---- F

Registration workflow C:
S -post system w/o id -> F
                         F <--Generates UUID and internally registers S-->
S <---Registered ID----- F


Fault Reporting Workflow A:
S -----Reports Fault ------> F
                             F <--Generates UUID-->
S <---Returns Fault UUID---- F


Fault Reporting Workflow B:
S <---Generates internal Fault id (or requests from F)--->

S -----Reports Fault with UUID------> F
S <-------Returns Fault UUID--------- F


Fault / System Update Workflow:
S <---Experiences Fault or System property change --->

S ---Reports fault or system with registered UUID ---> F
S <--------------Returns success message ------------- F



***MISSING FUNCTIONALITY***
There are a few elements that need to be added to these, or separate, APIs. This includes but is not limited to:
1) Security to protect against malicious actors with access
2) Alternate data formats (currently only supports JSON for both input and output)

