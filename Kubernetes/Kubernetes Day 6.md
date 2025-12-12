
**OpenIDConnect:**
- Kubernetes doesnt handle user identities and passwords directly and uses external identity providers like azure AD, google, etc
- OIDC is a standard protocol that lets Kubernetes trust those external identity providers
- There are two categories of users in a Kubernetes cluster - service accounts and user accounts 
- Kubernetes does not have objects which represent normal user accounts. Normal users cannot be added through normal api calls. 
- if a user presents a valid certificate signed by the cluster's CA is considered authenticated. 
- Username: common name field in the subject of the cert. From there, RBAC sub-system would determine whether the user is authorized to perform a specific operation on a resource. 
- It provides a centralized identity management system, SSO experience as well as enhanced security by leveraging JWT tokens for transmitting identity information. 

**OAUTH Terms:**
- Resource owner - the owner of the identity
- Client - application that wants to access data and want to perform actions on behalf of the resource owner 
- Authorization server - knows the resource owner and has an account in it 
- Resource server - API/Service that the client wants to use on behalf of the user. Sometimes its the same as the authorization server
- Redirect URI: url the authorization server will redirect the owner back to after granting permission to the client - callback url 
- Response type - type of information the client expects to receive - an authorization code 
- Scope - granular permission that the client wants 
- Consent - auth server takes the scope client is requesting and checks with the owner to see if these scopes can be granted to the client 
- Client ID: used to identify the client in the authorization server 
- Client Secret: the secret only auth server and the client knows which allows them to securely transfer data between one another 
- Authorization Code: short lived temporary code the auth server sends back to the client. Client sends back the auth code to the auth server along with the client secret for an access token 
- Access Token: key the client will use to communicate with the resource server.

**OAUTH Flow:**
- Resource owner(you) wants to allow a client(website) to access gmail to send email to all your friends 
- The website redirects you to the auth server (gmail) - includes the client id, redirect uri, response type, scope.
- Auth server verifies who you are with a login and presents you with a consent form for permission 
- Auth server redirects back to the client with the redirect uri along with a temp auth code 
- Client contacts the auth server directly and securely sends client id, client secret and auth code 
- Auth server verifies data and sends an access token back to the client. The client does not understand the access token 
- This access token is then used by the client to get info from the resource server (in this case gmail) and if it is valid, responds back with the contacts. 

**OIDC:**
- Thin layer above Oauth that adds functionality around login and profile info about the person who is logged in. Cannot work without the OAuth framework
- Instead of a key, it gives the client a badge - gives permissions as well as some information of who you are
- Authentication + identity. Which is why it is called an identity provider as it provides some information about the resource owner back to the client 
- Enables scenarios where one login can be used across multiple applications - SSO
- Scope=openid is specified and the same steps are followed as in oauth. When the client exchanges the auth code, client id and secret for the access token, it also gets an id token from the auth server. 
- Id token is a JWT token and gives the client some additional info such as id, name, when you logged in, expiration, tampering with jwt. Data inside jwt is called claims.

**Task: Create a deployment with init containers where the init container fetches a file from S3 and updates the local volume. Use only private S3 bucket**
- A pod in minikube should be able to access a private s3 bucket without hardcoding the aws access keys
- this is done by making kubernetes act as an OIDC provider, and issuing signed tokens to service accounts which are then attached to the pods 
- The OIDC makes AWS IAM trust the signed tokens, giving it temporary aws credentials 
- This temporary aws credentials allows a minikube pod to access a private s3 bucket. 
- in this case, kubernetes API server is the OIDC issuer which signs the service account tokens. AWS IAM is the resource server and the pod's service account is the identity that gets mapped to the IAM role. The client is minikube pod that wants to have access to aws's private bucket.
- You get info about iss (issuer), aud(who the token is meant for), sub(what the token is used for ) from discovery json that is put in the s3 bucket under openid-configuration 
- jwks_uri is the public key that aws will be using to verify the tokens from kubernetes
- Signing key - minikube has service account keys under the certs directory. The public key is exposed, converted to a JWK format and written into keys.json which is also pushed to a public s3 bucket
- Then, aws knows to trust tokens from the issuer url and get public keys at keys.json at that url 
- The IAM role with the OIDC trust - allows this role to be assumed by tokens which have sub-as the service account that you mean to attach to this pod, from this OIDC provider. 
- The service accounts with annotations tells the pod identity webhood - when the pod uses this SA give it a token which aws will be able to verify and link it to that iam role 
- The discovery.json file helps s3 pretend to be an OIDC discovery endpoint that describes your issuer and where the keys live. 
- The pod identity webhook expects cert-manager to generate a signed certificate and rejects requests until a valid certificate is mounted 
- in this case, the webhook watches pods that uses the service account with the iam role annotation and automatically injects a token that is short lived, is oidc compatible (jwt), and has the correct aud -> sts.amazonaws.com. This essentially makes minikube behave like eks

**Full workflow:**
- Pod in kubernetes - client 
- Kubernetes service account identity - resource owner 
- Kubernetes API server - Auth server 
- AWS STS - Resource server 
- S3 bucket - final protected resource 
- Pod wants to access a private bucket  -> pod gets a signed jwt service account token from the api server -> consent is replaced by an iam trust policy and iam permissions -> pod has an oidc 