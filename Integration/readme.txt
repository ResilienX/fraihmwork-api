The json file found in this folder is intended for use within Postman (Verified with version 9.7.1). Expected usage is to run individual requests, rather than folder-wise execution of the collection. This collection allows the publishing and receipt of API requests and responses to a FRAIHMWORK deployment, demonstrating how exchanges with FRAIHMWORK are done via the provided APIs and endpoints.

Before using, there is requirement for configuring the collection's variables with authentication information. Usage instructions are as follows:

1. Import the collection within Postman.
2. In the collection folder view, click the "FRAIHMWORK Integrator Starter Kit Collection", and select "Variables". 
3. Note, the clientId and clientSecret variables are not pre-filled as a matter of security. Contact ResilienX for this information for your deployment's authentication credentials {clientId and clientSecret} for using the collection of example API requests. Fill in the variables with the values provided by ResilienX. 
4. Make individual requests in Postman.
  a. The workflow for authenticating with the OAuth server is covered in the readme file one level up. Take a moment to familiarize yourself with this workflow; it should take you a few minutes to read through the information.
  b. To make a request from the collection once authenticated, click a request in the collection folder tree, and press the 'Send' button. 
  c. Make note of the HTTP response code indicating whether any problems have occurred.
