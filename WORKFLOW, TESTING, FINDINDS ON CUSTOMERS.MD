WHAT CUSTOMER CAN DO 

    CUSTOMER 
Search for stores or product

    BUSINESS OBJECT  

       SEARCH  

    OBSERVED WORKFLOW
     1) Customer enters search term 
     2) customers sends search request
     3) Application returns matchng results.


       CAPTURED ENDPOINT
            POST /v1/pages/search HTTP/2
        PURPOSE:For customers to search for restaurant and stores. 



    TESTING 
      INPUT VALIDATION
        - EMAPTY VALUE:
        - SERVER RESPONSES WITH: HTTP/2 400 Bad Request

         - INVALID /OPPOSITE VALUE:
         - SERVER RESPONSES WITH: HTTP/2 200 OK

         - SHORT VALUE: 
         - SERVER RESPONSES WITH: HTTP/2 200 OK

         - LONG VALUE: 
         - SERVER RESPONSES WITH:HTTP/2 200 OK

         - SPECIAL CHARACTERS TESTER
            SERVER RESPONSES WITH: HTTP/2 200 OK. 
             EXCEPT FROM BACKSLASH \ THAT SHOWED: 400 BAD REQUEST


      FINDINGS: 
          The endpoint accepts serveral  tested input variations, including                                                                     short,long,invalid/opposite,and most special-character values.
           Empty input and backslash input resulted in 400 Bad Request. No obvious input-validation vulnerability was                            identified


         PARAMETER MANIPULATION

         Empty parameter:
          SERVER RESPONSES WITH : HTTP/2 400 Bad Request


        - LONG PARAMETER 
         - SERVER RESOINSES WITH: HTTP/2 BAD REQUEST

          - INVALID /OPPOSITE PARAMETER
          - SERVER RESPONSES WITH: HTTP/2 BAD REQUEST


       FINDINGS
           The application rejected unsupported parameter and accpted only the expected parameter.




      HTTP METHOD TESTING 

         -The HTTP method was change from POST to GET to PUT to PATCH to DELETE. 
          - SERVER RESPONSES WITH: HTTP/2 403 Forbidden 


       FINDINGS 
         The endpoint only support POST mehtod unsupport method was correctly rejected. No vulnerability was identified.


       OVERALL FININDS ON THIS ENDPOING 
         NO SECURITY VULNERABILITIES WERE IDENTIFIED ON THIS ENDPOINT DURING TESTING OF ITS OBSERVED FUNCTIONS, PARAMETERS,INPUT VALIDATION, AND HTTP MEHTOD.






CUSTOMERS 
    View product details

       BUSINESS OBJECT 
       product details 

          OBSERVED BUSINESS WORKFLOW 
           1) User search for product 
           2) Application displays matching products 
           3) User select product 
           4) Application displays the product details.
           5) User click on start ordering


     START ORDERING
       1) Application displays add to order
       2) Item added into order basket


        CAPTURE ENDPOINT
        POST /order-xp/v1/baskets HTTP/2

     PURPOSE 
         Returns the number of items currently in the user's basket 


     BASELINE REQUEST
        - STATUS: HTTP/2 200 OK
        - SERVER RETURNS 


     AUTHENTICATION TESING

        - Remove the authentication: Brear
        - server responses with: HTTP/2 401 Unauthorized

         FINDINGS 
            AUTHENTICATION WAS PROPERLY ENFORCE, NO VULNERABILITY  WAS IDENTIFIED


     HTTP METHOD TESINT 
        - THE REQUEST WAS CHANGE FROM POST TO GET
        -  SERVER RESPONSED WITH: HTTP/2 403 Forbidden

          - POST REQUEST WAS CHANGE TO PUT 
          - SERVER RESPONSED with: HTTP/2 400 Bad Request

         - POST REQUEST WAS CHANGED TO PATCH 
         - SERVER RESPONSED WITH: HTTP/2 400 Bad Request

         - POST REQUEST WAS CHANGED TO DELETE 
         - SERVER RESPONSED WITH: HTTP/2 400 Bad Request


     FINDINGS 
        THIS ENDPOINT ONLY SUPPORT POST METHOD, UNSUPPORT HTTP METHOD WAS CORRECTLY REJECTED BY THE SERVER.
        NO VULNERABILITY WERE IDENTIFIED. 

         
         INPUT VALIDATION 
         NORMAL VALUE: COUNT:1

          - count = 0         
          - Server HTTP/2 400 Bad Request 
          - Observation: The server rejected the value because count must be greater than 0


          - COUNT = 99999 
          - SERVER RETURNS : HTTP/2 200 OK
          - OBSERVATION: Server acccpted the large value. Verify whether the value was actually processed/accepted by the application

          - COUNT = ACB
          - SERVER RETURNS: HTTP/2 400 Bad Request
           OBSERVATION: The applicaton rejected the alphabetic input and returned 400 BAD REQUEST 


          - COUNT = EMAPTY VALUE 
          - SERVER RETURNS: HTTP/2 400 Bad Request
           - OBSERVATION: The server rejected the empty input 

           FINDINGS 
             NO vulnerability were identify during this tesing.The server accepted values that was greater than 0 and rejected                  alphabetic values.

          PRICE 
         NORMAL VALUE: 250 

        - PRICE = ABC 
        - SERVER RETURNS: HTTP/2 400 Bad Request
        - OBSERVATION: The application rejected the alphabetic input 


         - PRICE =99999 
         - SERVER RETURNS:HTTP/2 200 OK
         - OBSERVATION: The server accepted larger values. 

         OVERALL OBSERVATION: THE APPLICATION  ACCEPT ANY NUMBERIC VALUES BUT NO PRICE MANIPULATION WAS IDENTIFIED .


       FINDINGS 
         NO VULNERABILITY WAS IDENTIFIED DURING THIS PRICE MANIPULATION.


HEADER MANIPULATION 

      Remove the referer Header value: 
       Server returns: HTTP/2 200 OK

       Remove the orgini header value: 
      server retuns: HTTP/2 200 OK


     FINDINGS 
      THE APPLICATION DOES NOT NEED THE HEADER VALUES TO BE PRESENT.
       NO VULNERABILITY WAS IDENTIFIED.




CUSTOMER 
     ADD PAYMENT METHOD 

       BUSINESS OBJECT 
        PAYMENT METHOD/CARD

     BUSINESS WORKFLOW
       1 USER CLICK ON THIS PROFILE
      2 USER CLICKS ON PAYMENT METHOD 
       3 USER CLICKS ON ADD NEW CARD 
       4 APPLICATION DISPLAYS CARD FORM 

STATE:   Blocked by client-side/input validation because of valid card is required 
         No testing was done


