# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
- /update-role checks for the user's role using ```if (user.role !== 'admin')``` but this does not
properly authenticate the user. The userId is passed by the request body without any form of
verification that the user sending the request is actually the logged-in user or has real privileges.
- Because there is no session management to authenticate the user, anyone can send a POST request
- to /update-role and update any user's role if they know the userId.

2. Briefly explain how a malicious attacker can exploit them.
- The attacker can POST request to /update-role with any user id and new role since there is no
security. The userId and newRole are passed in the request body and there is no authentication 
to verify the identity of the user sending the request.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
- Authentication is added to **secure.ts** that makes sure that a session is made and that the
session is authorized to make the changes ```if (!req.session.userId)``` The session middleware is 
important for handling these user sessions.
- It also checks if the user's role is 'admin' ``` loggedInUser.role !== 'admin'```
