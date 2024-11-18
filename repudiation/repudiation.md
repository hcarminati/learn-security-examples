# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
- There is no way for the user to repudiate their action. 
- The server gets the message without authentication, so an attacker could submit a message to the
server as if they were someone else and there would be no way to prove or verify that it was the 
attacker who sent the message since the server does not keep logs to track who sends which messages.

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
- The vulnerability is addressed by adding authentication mechanism to /get-messages so it requires
the user to identify themselves.
- Messages are also logged in /send-message so that it can be traced and makes it harder for 
attackers to repudiate their actions.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
- The design pattern used is the mediator pattern that allows you to intercept requests before
they reach the main logic of the application. 
- Before processing the req, the middleware is used for logging requests; it logs the HTTP method, 
the route accessed, and the user’s IP address. This makes sure that there is a traceable record of
each action so that it is harder for the attacker to repudiate their actions. 