# Security attacks

## Cross Site Scripting (XSS)

Cross Site Scripting (XSS) is a security vulnerability which enables an attacker to inject malicious scripts (JavaScript) into web pages viewed by other users. The attacker can use this vulnerability to steal user credentials, session tokens, or other sensitive information. The attacker can also use this vulnerability to perform actions on behalf of the user. XSS is a type of code injection attack.

### Types of XSS

1. **Reflected XSS**: The attacker injects a malicious script into a vulnerable web page, which is then reflected back to the user. The attacker can send a link to the victim, and when the victim clicks on the link, the malicious script is executed.
2. **Stored XSS**: The attacker injects a malicious script into a vulnerable web page, which is then stored on the server. The script is executed whenever a user visits the page.
3. **DOM-based XSS**: The attacker injects a malicious script into a vulnerable web page, which is then executed by the user's browser. The script is not sent to the server, so it is not stored on the server. An example of a DOM-based XSS attack is when the attacker injects a SVG image with a malicious script into a web page that gets instantly executed by the browser.

### Example

```python
from flask import Flask, request
from mako.template import Template

app = Flask(__name__)

# Mako template with NO automatic escaping
mako_template = Template("""
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>Mako XSS Demo</title>
</head>
<body>
    <h1>Hello ${name}!</h1>
    <form method="POST">
        <input type="text" name="name" placeholder="Enter your name (or malicious script)"/>
        <button type="submit">Submit</button>
    </form>
</body>
</html>
""")

@app.route("/", methods=["GET", "POST"])
def index():
    # Default to "World!" if no name provided
    name = "World!"
    if request.method == "POST":
        # Here, we directly insert user input into the Mako template
        # NO escaping -> vulnerable to XSS
        name = request.form.get("name", "World!")

    # Mako renders the template without autoescaping, so malicious <script> can run
    return mako_template.render(name=name)

if __name__ == "__main__":
    app.run(debug=True)
```

## SQL Injection

SQL Injection is a security vulnerability which enables an attacker to execute arbitrary SQL queries on a database. The attacker can use this vulnerability to steal user credentials, session tokens, or other sensitive information. The attacker can also use this vulnerability to perform actions on behalf of the user. Like XSS, SQL injections are a type of code injection attack.



## Cross Site Forgery Request (CSRF)

CSRF (also known as one-click attack or session riding) is malicious exploit of a website where an attacker tricks a user into performing actions they did not intend to. It is a type of attack that occurs when a malicious website, email, blog, instant message, or program causes a user's web browser to perform an unwanted action on a trusted site when the user is authenticated. Usually this happens via cookie-based authentication.
