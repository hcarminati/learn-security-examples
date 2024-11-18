# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out?

## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
- There cross-site scripting happening and the app is stealing the user's cookie. By clicking the 
link in __mal-steal-cookie.html__, the malicious server (__mal.ts__) responds with an HTML page 
containing Javascript. The JavaScript steals the user's session cookie from localhost:8000
Because the cookie is scoped to localhost, the session cookie from localhost:8000 could be with 
any requests sent to any localhost:<port>. If the browser has a valid session cookie, the 
JavaScript logs the contents of document.cookie to the browser's console controlled by the
attacker's server. The attacker then just makes a POST request (/sensitive) to localhost:8000
and the user's browser will process the request because the POST includes the user's session cookie.

2. Briefly explain different ways in which vulnerability can be exploited.
- With cross-site scripting, the vulnerability can be exploited by injecting malicious JavaScript 
into the page that accesses the user's cookie and sends it to the attacker's server which the 
attacker can then use to perform actions on behalf of the user. 
- The attacker can also make a request to perform unauthorized actions for the user once they have
the user's cookie. If the user visits a page controlled by the attacker, it automatically submits 
the request to the endpoint. As seen in /sensitive __insecure.ts__, the malicious form is 
automatically submitted and since the victim is already authenticated in the vulnerable server, the 
server will treat the request as if it was initiated by the logged-in user.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
- **insecure.ts** is vulnerable to cross-site scripting attacks because it does not set the httpOnly 
and secure flags on the session cookie. httpOnly makes sure that the session cookie cannot be 
accessed with JavaScript and sameSite makes sure that the cookie is not sent with cross-site requests
which means the cookie will only be sent when the request is coming from the same origin. 