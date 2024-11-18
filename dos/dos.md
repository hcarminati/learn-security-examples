# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
- The vulnerability was a NoSQL injection that lead to a denial of service attack. The id query param
from the URL is passed directly into User.findOne() without any sanitization which allows an 
attacker to manipulate the query and inject harmful queries. 

2. Briefly explain how a malicious attacker can exploit them.
- Because the id is passed directly into the query, the attacker can use other mongo operations to
can lead to expensive queries and cause the server to crash. The system does not implement any rate
limit si the attacker sends requests to overload the system and it crashes.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
- the **secure.ts** code implements a rate limiter that prevents the abuse by limiting the number of
requests a user can make and returning a try later message if it is overloaded. There is also a try
catch around the query that handles this error that could be thrown and responds with a 500 which 
can prevent detailed error messages from leaking. 