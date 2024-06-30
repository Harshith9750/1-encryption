# 1-encryption
Building a web application for text encryption and decryption involves using cryptographic algorithms to securely transform text data. Below is a high-level outline for creating such an application, using Python for the backend and HTML/CSS/JavaScript for the frontend. We'll use the Flask framework for the backend and the Cryptography library for encryption and decryption.

Steps to Build the Web Application
Setup Environment:

Install Flask: pip install Flask
Install Cryptography: pip install cryptography
Create the Flask Application:

Create a directory for the project and navigate into it.
Create a Python file (e.g., app.py).
Backend (Flask):

python
Copy code
from flask import Flask, render_template, request, redirect, url_for
from cryptography.fernet import Fernet

app = Flask(__name__)

# Generate a key for encryption and decryption
# You must use this same key for decryption
key = Fernet.generate_key()
cipher_suite = Fernet(key)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/encrypt', methods=['POST'])
def encrypt():
    plaintext = request.form['plaintext']
    encrypted_text = cipher_suite.encrypt(plaintext.encode()).decode()
    return render_template('index.html', result=encrypted_text, plaintext=plaintext)

@app.route('/decrypt', methods=['POST'])
def decrypt():
    encrypted_text = request.form['encrypted_text']
    decrypted_text = cipher_suite.decrypt(encrypted_text.encode()).decode()
    return render_template('index.html', result=decrypted_text, encrypted_text=encrypted_text)

if __name__ == '__main__':
    app.run(debug=True)
Frontend (HTML):

Create a templates directory and inside it, create an index.html file.
html
Copy code
<!DOCTYPE html>
<html>
<head>
    <title>Text Encryption</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            background: white;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        input[type="text"], input[type="submit"] {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Text Encryption</h1>
        <form method="POST" action="/encrypt">
            <input type="text" name="plaintext" placeholder="Enter text to encrypt" required>
            <input type="submit" value="Encrypt">
        </form>
        <form method="POST" action="/decrypt">
            <input type="text" name="encrypted_text" placeholder="Enter text to decrypt" required>
            <input type="submit" value="Decrypt">
        </form>
        {% if result %}
            <h2>Result:</h2>
            <p>{{ result }}</p>
        {% endif %}
    </div>
</body>
</html>
Explanation:
Backend:

Flask Application: The backend uses Flask to handle HTTP requests.
Cryptography: The cryptography.fernet module is used to handle encryption and decryption. The Fernet class provides symmetric encryption and decryption.
Routes:
/: The home route renders the form.
/encrypt: Handles the encryption of plaintext.
/decrypt: Handles the decryption of encrypted text.
Frontend:

HTML Form: The form allows users to input text for encryption and decryption.
Result Display: After processing, the result is displayed on the same page.
Running the Application:
Save the app.py file and the index.html file in the appropriate directories.
Run the Flask application by executing python app.py.
Open a web browser and navigate to http://127.0.0.1:5000/ to use the application.
Security Note:
This example uses a simple form of encryption for demonstration purposes. In a production environment, key management, secure storage, and more robust algorithms (like AES) should be considered to ensure security.





