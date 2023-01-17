# Auth0's API Documentation

## **Contents**

• [Overview](#overview)
• [Why Auth0](#justifications)
• [Process Flow - Proof Key for Code Exchange - User experience](#process-flow)
• [What does it do?](#actions)
• [Site Implementation Details](#implementation)

## [Overview](#overview)

The purpose of this document is to clarify some of the essential considerations that informed the migration of the Underdog Devs platform from using Okta to using Auth0 for user authentication on its site. It includes high-level explanations of key concetps and processes, references to relevant internal and external sources of truth on the services mentioned, and an explanation of how and where it has been implemented across Underdog Devs repositories.

Auth0's Authentication API allows you to manage all aspects of user identity, offering endpoints that allow users to sign up, log in/out, access other APIs and more.

When testing endpoints, ensure requests are sent using `HTTP`

## [Why Auth0?](#justifications)

Okta and Auth0 share similar features, such as single sign-on, identity management, and user governance tools. In fact, Auth0 was acquired by Okta in May 2021, with the former becoming an independent business unit within the latter. While Auth0 may appeal to small to medium sized enterprises with its forever free tier, Okta is attractive to larger organisations with thousands of users and the need for more advanced security protocols. Below are a few more contrasts between the services provided by these two platforms:

### Auth0:

- Primarily focused on authentication, providing multi-factor authentication tools and supporting a wide range of identity providers, including social media logins
- Does not support biometric authentication or zero trust networking
- Designed with application builders in mind, providing simplicity and strong "identity management protocols"
- More developer-centric, offering greater flexibility and customisation options
- Can handle up to 50,000 external users and over 5,000 internal employees
  [*Free tier supports up to 7,000 active users and unlimited logins*]

### Okta:

- Supports [zero trust networking](https://www.cisco.com/c/en/us/solutions/automation/what-is-zero-trust-networking.html)
- Supports biometric authentication
- Offers a more complex and secure system that is geared towards IT professionals
- Primarily supports enterprise identity providers such as Active Directory
- Has no limit regarding the number of active users

## [What does Auth0 do?](#process-flow)

### Simplified:

After Auth0 has been implemented in an application, users who try to login will be redirected to a customisable login page where they can input their credentials. If authenticated, Auth0 redirects the user back to your app, returning a JSON Web Token (JWT) with their authentication and user information.

### Detailed:

1. User clicks Login within application.
2. Auth0's SDL creates a cryptographically random `code_verifier` and from this generates a `code_challenge`.
3. Auth0's SDK redirects the user to the Auth0 Authorization Server along with the `code_challenge`.
4. Your Auth0 Server redirects user to login and authorization prompt.
5. User authenticates using one of the configured login options and may see a consent page listing the permissions Auth0 will give to the application.
6. Your Auth0 Authorization Server stores the `code_challenge` and redicrects the user back to the application with an authorization `code`, which is good for one use.
7. Auth0's SDK sends this `code` and the `code_verifier` (step 2) to the Auth0 Authorization Server.
8. Your Auth0 Authorization Server verifies the `code_challenge` and `code_verifier`.
9. Your Auth0 Authorization Server responds with an ID token and access token
10. Your application can use the access token to call an Api to access information about the user.
11. The API responds with requested data.

## [How does Auth0 work?](#actions)

The auth0-react.js library can be used to implement authentication and authorization in React apps with Auth0. It provides a custom React hook and other Higher Order Components to help secure apps in fewer lines of code. There are three ways to authenticate with this API:
_OAuth2 token_
By sending a valid Access Token in the `Authorization` header, you can then make requests to an endpoint (protected routing?)
_Client ID and Client Secret_
By sending your Client ID and Client Secret information in the request JSON body. This option is only available for confidential applications - such as those able to store credentials securely without exposing them to unauthorized parties.
_Client ID_
For public applications that cannot securely hold credentials (SPAs or mobile apps), some endpoints are accessible using only Client ID.

For greater context, check out the [Auth0 Authenticatin API docs website](https://auth0.com/docs/api/authentication#introduction)

## [Underdog Devs Site Implementation Details](#implementation):

Auth0 is implemented via the auth0Middleware which lives in the `underdog-devs-be-a/api/middleware/auth0Middleware.js` file in the backend repository. Opening this file, we are particularly interested in the _verifyJWT_ method, which executes an express middleware to verify JSON Web Tokens - this is the method that is called in the _authRequired_ method which is responsible for protected routing.

When invoked, _verifyJWT_ returns a JWT config object, and also specifies an 'unless' condition which takes an object containing a path parameter which is an array. The paths specified in this array represent public endpoint routes which require no authentication (e.g. `/application/new/:role`, which is the endpoint used to submit new mentor/mentee applications). By specifying exceptions to protected routing in this way, individual endpoints do not need to be targeted to implement or remove authentication.
