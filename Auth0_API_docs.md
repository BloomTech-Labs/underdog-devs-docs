#Auth0's API Documentation

##**Contents**
• [Overview][]
<!-- • [What is Auth0 - Authentication vs Authorisation][] -->
• [Strengths/Why Auth0][]
• [Process Flow - Proof Key for Code Exchange - User experience][] <!-- Better name -->
• [What does it do?][]

<!-- The purpose of this document is to clarify some of the essential considerations that informed the migration of the Underdog Devs platform from using Okta to using Auth0 for user authentication on its site. It includes high-level expla**nations of key concetps and processes, and links to relevant internal and external sources of knowledge on the services mentioned. -->

##[Overview][]
Auth0's Authentication API allows you to manage all aspects of user identity, offering enpoints that allow users to sign up, log in/out, access other APIs and more.

All requests must be secure, i.e. `HTTPS`, not `HTTP`


<!-- ##[What is Auth0?][]
Auth0's Authentication API allows you to manage all aspects of user identity, offering enpoints that allow users to sign up, log in/out, access other APIs and more. -->

##[Why the switch?][] <!-- Auth0 vs Okta -->
Okta and Auth0 share similar features, such as single sign-on, identity management, and user governance tools. In fact, Auth0  was  acquired bybOkta in May 2021, with the former becoming an independent business unit within the latter. While Auth0 may appeal to small to medium sized enterprises with its forever free tier, Okta is  attractive to larger organisations with thousands of users and the need for more advanced security protocols. 

<!--- Insert table to show differences -->
###Auth0:
• Has multi-factor authentication tools
• Does not support biometric authentication or 
• Designed with application builders in mind, providing simplicity and strong "identity management protocols"
• Can handle up to 50,000 external users and over 5,000 internal employees[*][5]
[*Free tier supports up to 7,000 active users and unlimited logins*][5]

###Okta:
• Supports [zero trust networking](<!-- Link to article -->)
• Supports biometric authentication
• Offers a more complex and secure system,
• Has no limit regarding the number of active users

##[What does Auth0 do?][]
###Simplified:
After Auth0 has been implemented in an application, users who try to login will be redirected to a customisable login page where they can input their credentials. If authenticated, Auth0  redirects the user back to your app, returning a JSON Web Token (JWT) with their authentication and user information.

###Detailed:
1. User clicks Login within application.
2. Auth0's SDL creates a cryptographically random `code_verifier` and from this generates a `code_challenge`.
3. Auth0's SDK redirects the user to the Auth0 Authorization Server [`/authorize` endpoint]() along with the `code_challenge`.
4. Your Auth0 Server redirects user to login and authorization prompt.
5. User authenticates using one of the configured login options and may see a consent page listing the permissions Auth0 will give to the application.
6. Your Auth0 Authorization Server stores the `code_challenge` and redicrects the user back to the application with an authorization `code`, which is good for one use.
7. Auth0's SDK sends this `code` and the `code_verifier` (step 2) to the Auth0 Authorization Server ([`/oauth/token` endpoint] ()).
8. Your Auth0 Authorixation Server verifies the `code_challenge` and `code_verifier`.
9. Your Auth0 Authorization Server responds with an ID token and access token
10. Your application can use the access token to call an Api to access information about the user.
11. The API responds with requested data.


##[How does Auth0 work?][] <!-- Link to Bennet's file? -->
The auth0-react.js library can be used to implement authentication and authorization in React apps with Auth0. It provides a custom React hook and other Higher Order Components to help secure apps in fewer lines of code. There are three ways to authenticate with this API:
*OAuth2 token*
By sending a valid Access Token in the `Authorization` header, you can then make requests to an endpoint (protected routing?)
*Client ID and Client Secret*
By sending your Client ID and Client Secret information in the request JSON body. This option is only available for confidential applications - such as those able to store credentials securely without exposing them to unauthorized parties.
*Client ID*
For public applications that cannot securely hold credentials (SPAs or mobile apps), some endpoints are accessible using only Client ID.