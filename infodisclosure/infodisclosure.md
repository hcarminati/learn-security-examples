# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
- The route to authenticate user (/userinfo) is vulnerable to NoSQL injection and the vulnerable 
code is directly using user-provided values in the query without any validation or sanitization.
This allows the user to inject a malicious query object like username[$ne]= in the request which 
would modify the MongoDB query logic.

2. Briefly explain how a malicious attacker can exploit them.
- The attacker creates malicious requests to change the sql query to do that they want it to. 
In the case of ```http://localhost:3000/userinfo?username[$ne]=```, the query bypasses authentication
and the query used would be ```User.findOne({ username: { $ne: '' } })``` which would get any user
that has the username that is not an empty string which would return all the users in the database 
with their passwords.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
- They extract the username from the query parameter and sanitized the username input to prevent
the NoSQL injection. The inputted is checked to see if it is a string nad anny non alphanumeic 
characters are removed. 