GUEST 
            
    LOGIN 

	BUSINESS OBJECT 
	   user account 

     OBSERVER APPLICATION WORKFLOW
          1 gust clicks log in 
           2 login page is displayed 
           3) user selects a login method(Email, Google, Apple, or facebook)
           4) user selects CONTINUE WITH GOOGLE 
           5) user authenticates with their google account 
           6) wolt requests a phone number for verification 
           7) users enter their phone number 
           8) A verification code is sent via WhatsAPP/ SMS
           9) User enters the verification code
           10) wolt creates/authenticates the customer account 
           11) user is redircted to the customer dashboard/home page.




   CAPTURE ENDPOINTS 
            GET /v1/user/me HTTP/1.1
                PURPOSE: RETURNS THE LOGGED-IN USER'S PROFILE.        
            
            TESTING 
             Baseline request 
                - Status: HTTP/2 200 OK
                - Returns the authenticated users profile and account information.

                AUTHENTICATION TEST
                - Remove the authorization: Bearer header 
                - Server returned HTTP/2 401 unauthorized.
                -  Authentication is properly enforced. 
                - FINDINGS: NONE  

                 IDOR TESTING
                 - No user-controllled identifier in the request.Direct IDOR testing is not applicable

                 SENSITIVE DATA EXPOSURE TESTING: 
                   - Remove authorization header.
                   - Reviewed response for sensitive data exposure. 

                  FINDING:
                   - Authentication is enforced.Removing the authorization header returned HTTP 401                        Unauthorized.
                   - No sensitive data exposure was identified. 
                   - The endpoint returned only expected profile information for the authenticated                          user. 

                  HTTP METHOD TESTING
                    - Change the request method from GET TO POST,GET TO PUT, GET TO DELETE 
                    - Serve responses with 404 page not found 

                      FINDING: 
                         - The endpoint only supports the GET method.
                         - Unsupported HTTP methods were correctly rejected. 
                         - No vulnerability identified.

                    HEADER MANIPULATION TESTING 
                        - Remove the origin Header value. 
                         - server returned HTTP/2 200 OK

                          - Remove the referer header value. 
                           - Server returned HTTP/2 200 OK 

                      FINDING: 
                       - The endpoint does not rely on the Origin oe Referer headers forauthentication 
                        or authorization.
                       - The request was processed successfully without these headers.
                       - No vulnerability identified.

                 AUTHORIZATION HEADERS MANIPULATION
                   - Replaced the valid bearer token with an invalids tokens.
                   - Server response with 401 unauthorized.


                    FINDINGs
                     - The server correctly rejected invalid authentication tokens. 
                     - Authentication validation is properly enforced. 
                     - No vulnerability identified.
                    


POST /auth-api/v2/token 
                   PURPOSE: OBTAINS AN AUTHENTICATION TOKEN AFTER SUCCESSFUL LOGIN

                TESTING 
                  
            
            GET /v3/user/me/payment_methods HTTP/2 
            PURPOSE: Returns the logged customers saved payment methods

            TESTING 
             BAELINE REQUEST 
                - Status: HTTP/200 OK
                - Returns the authenticated user payment method 


               AUTHENTICATION TEST
                 - Remove the authentication: Bearer header
                 - Server responses with: HTTP/2 401 Unauthorized

               FINDINGS: AUTHENTICATION IS PROPERLY ENFORCED 

               
               SENSITIVE DATA EXPOSURE TESTING
                 - Remove the authentication header
                 - Reviewed responses for sensitive data exposure

                 FINDINGS 
                    Authentication is enforced,removing the authentication headers returns HTTP/2 401                             Unauthorized. 
                     - No sensitive data exposure was identified
                     - The endpoint returned secure information. 


              HTTP METHOD TESTING
                - The HTTP method was change from GET TO POST, GET TO PUT, GET TO PATCH, GET TO DELETE
                - The server returned: HTTP/2 405 Method Not Allowed 

              FINDINGS: 
                 The endpoint only support the GET method.
                 Unsupported HTTP method were correctly rejected 
                  NO vulnerablity were identified


             HEADER MANIPULATION TESTING
                - Removed the origin header value.
                - Response: HTTP/2 200 OK 

                 - Remove the orign header completely.
                 - Respone: HTTP/2 403 Forbidden 
              

                  - Remove the referer value. 
                  - response: HTTP/2 200 
                   
				   - Remove the referer header completely 
				   - Response: HTTP/2 403 FORBIDDEN
                  
                  
                  
                  OBSERVATION:
                     The 403 response was generated by Cloudfront
                
                  FINDINGS: 
                   The application requires the orgin header to be persent.Request without the origin header are rejected with HTTP 403 Forbidden. No vulnerability were identified. 







GUEST 
     SEARCH FOR JOBS 

        BUSINESS OBJECT
             JOBS 

              OBSERVE APPLICATION WORK FLOW
                 1 USER CLICKS SEARCH FOR JOBS 
                 2 USER SEARCHES BY JOB TITLE (OPTIONAL)
                 3  USER FILTERS BY TEAM (OPTINAL)
                  4 USER SELECT LOCATION
                  5 THE APPLICATION DISPLAY MATCHING JOBS 
                  6 USER SELECTS A JOB LISTING 
                  6 USER APPLY 


       APPYLY FOR JOB  
          BUSINESS OBJECT: JOB APPLICATION 

      OBSERVED APPLICATION WOKRFLOW
            1 USER CLICKS APPLY 
             2 APPLICATION DISPLAYS THE APPLICATION FORM 
             3 USER ENTERS FIRST NAME 
            4  USER ENTERS LAST NAME 
             5 USER ENTERS EMAIL ADDRESS 
             6  USER ACCEPTS THE RECRUITMENT PRIVACY STATMENT
              7 USER ANSWERS SCREENING QUESTION 
               8 USER ENTERS LOCATION 
               9 USER UPLOAD A CV/ resume 
               10 user upload a cover letter 
              11  USER SUBMITS THE APPLICATION 
              13 APPLICATION DISPLAYS A CONFIRMATION OR SUCESSMESSAGE. 
          

      CAPTURE ENDPOINT 
         POST /api/v2/jobs/8051073/apply HTTP/2
       PURPOSE: CREATES A JOB APPLICATION 
      
      
      
      
      TESTING
        BASELINE REQUEST: 
          - STATUS: HTTP/2 201 Created
          - Returns the application creation 


       IDOR TESTING
          - Modified the job_id parameter to several other values
          - Existing job IDs returned HTTP 201 Created.
          - invalid or non-existing IDS returned HTTP 500 INTERNAL SERVER ERROR
          - The endpoint appears to accept applications for valid job postings
          - No unauthorized access to another user's data or resosurces was observerd

            FIDNING: 
               No IDOR identified during testing 


      INPUT VALIDATION
         - parameter: first_name
         - Removed the value. 
         - server responses with HTTP 400 Bad Request
         - The server enforces that first_name is a required field 
         - No improper validation or security issue identified during testing

           PARAMETER: LAST_NAME
             - Removed the value.
             - Server responses with HTTP 400 Bad Request 
             - The server enforces that last_name is a required field
             - No improper validation or security issue identified during testing 


          PARAMETER: EMAIL 
             - Removed the entirely value 
             - The server responses with:  HTTP/2 400 Bad Request
             - The server correctly enforces that the email field is required.


           TESTED IT WITH ANOTHER VALID EMAIL
              - Replaced the email with a different valid email address
              - server responses with: HTTP 201 Created 
              - The server accept valid email addresses. 


            FINDINGS 
               The server properly validate the email parameter.Emply email values are rejected with HTTP 400 REQUEST,While valid                     email addresses are accepted. NO improper input validtion issues were identified during testing.

            PARAMETER: PHONE NUMBER 
              -  Remove the entire value :
              -  server responses with HTTP/2 201 created
               - opposite value/invalid value:
               - server responses with HTTP/2 400 Bad Request
               -  Short value: 
               -  server responses with HTTP/2 201
               -  long value: 
               -  server responses with HTTP/2 201 created

              FINDINGS 
               The application rejected invalid (non-numeric) phone number values but accepted       
               empty,short, and long numberic values. No immediate security impact was identified 
               during testing.


              HTTP METHOD TESTING 
               POST TO GET 
               PUT 
               PATCH 
               DELETE 

            SERVER RESPONSES: 
                - POST TO GET: 403 FORBIDDEN 
                - POST TO PUT : 405 METHOD NOT ALLOW
                - PATCH : 405 METHOD NOT ALLOW 
                - DELETE: 405 METHOD NOT ALLOW


               FINDINGS 
                 The endpoint only support the POST METHOD 
                 Unsupported HTTP METHOD are correctly rejected.
                 No vulnerability was identified


           HEADER MANIPUTLATION

        Remove the referer Header value: 
         Server responses with: HTTP/2 201 Created


	    Remove the referer Header completely: 
	      server responses with:HTTP/2 400 Bad Request


         Remove the orgin Header value: 
          server responses with: HTTP/2 201 created 

         Remove the origin Header completely: 
          server responses with: HTTP/2 400 Bad Request


         FINDINGS 
          The application requires the ORIGIN AND REFERER HEADER TO BE PRESENT.
           Request without the HEADERS are rejected with 400 BAD REQUEST. 
         NO vulnerability were identified. 




APPLY TO BEOMONE WOLT COURIER PARTNER            
                

     BUSINESS OBJECT 
         COURIER PARTNER 


    OBSERVE APPLICATION WORKFLOW
            
        1 User click to become a Wolt courier partner
        2 User click apply now 
        3 The application display the application form 
         4 User enters first name 
         5 User enters last name 
         6 User enters Email address
         7 User enters language spoken
          8 User enters date of birth 
          9 User enters Phone number 
          10 User select delivery Country 
          11 User select delivery city 
          12 User select delivery vehicle
          13 User read and agree with the [User Terms of Service]
        
            SUBMISSION STATUS: Unable to complete the final submission becauherefore, the actual application-submission request could not be observe  during this attempt





GUEST 
     
       APPLY TO BECOME A MERCHANT PARTNER 

      BUSINESS OBJECT
          MERCHANT PARTNER

     OBSERVED BUSINESS WORKFLOW
        1 User apply to become a merchant
         2 User click on start for free or login


      USER CLICK ON START FOR FREE
          WORKFLOW 

         1 The application displays a form
         2 User enter first name 
          3 User enter last name
          4 User enter business email
          5 User select country 
          6 User enters store name or location
                 User select business category
                 User select number of location they have the business.



     CAPTURE ENDPOINT 
         POST /api/merchant-onboarding/merchant-admin/inbound-merchant HTTP/2
     PURPOSE 
         Creates/submits merchant onboarding information 


     TEST RESULT: Request replayed in Burp Repeater. Server returned 403 Forbidden with "INVALID hCAPTCHA". the endpoint could not be further tested because the request failed CAPTCHA validation.



GUST 

    Apply for wolt drives 

	  BUSINESS OBJECT 

         Wolt drives  

     OBSERVED BUSINESS WORKFLOW 
          1 User clicks for wolt drives
          2) User click on get started
          3) Application displays sign up from


   sign up form 
     User enters name 
     User enters  work email 
     User enters company name 
     User enters phone number 
     User select  company size 
     User select country 
     User sign up as a restaurant or courier 

    OBSERVERATION: This function is connected to wolt account creation/sign up. An existing wolt account is already available, so the account-creation flow was not tested. 



    
          
