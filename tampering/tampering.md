# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
- The vulnerability is cross site scripting. The app renders the user input directly into the HTML
- output without sanitization which makes it vulnerable to malicious script injections. The attacker
- inputs a script instead of a name and that script is executed in the browser.

2. Briefly explain how a malicious attacker can exploit them.
- The attacker can modify the content of the webpage by injecting JavaScript directly into the HTML
through the input form. Many scripts can be injected like ones to also steal session cookies.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
- **secure.ts** does not have the same vulnerabilties because the user input is sanitized which 
prevents the browser from interpreting the input as code. 
- When we set the name in **secure.ts**, we use the method ecapeHTML in /register because we dont
want HTML script to be part of the string, so we transform it by replacing every special character 
we see with its string equivalent.
- First make a list of what you allow and everything else is going to be rejected, that is what
sanitization is about if you are dealing with untrusted inputs you need ot make sure they have
 the format that prevents tampering attacks.
- Need to make sure that it is in the exact format we tell it, only alpha numeric. 
- Every sanitizing rule will depend on domain.
- Value of input if it comes from an untrusted source is important and we need to identify untrusted 
sources and sanitize them.
