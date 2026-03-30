## Challenge Writeup

The challenge presented a web application called **CTF7 Card Studio**, a digital greeting card generator where users could input a recipient name and a custom message to render a styled card.

### Step 1: Identifying the Vulnerability
To test for template injection, a basic SSTI (Server-Side Template Injection) payload was used:<br>
```bash
{{ 7*7 }}
```

This payload was entered into both the **recipient name** and **message** fields. Upon rendering, the output evaluated the expression instead of displaying it as plain text, confirming the presence of an SSTI vulnerability.

### Step 2: Exploitation
After confirming SSTI, multiple payloads were tested to explore the template context. Eventually, the following payload was used:<br>
```bash
{{ config }}
```

This exposed internal configuration data within the rendered output.

### Step 3: Retrieving the Flag
While inspecting the output of the payload, the flag was found appended at the end of the rendered content.
