# PROJECT 6
## FLASK API PROJECT

### PROJECT GOAL

The goal of this project is to demonstrate and provide a foundation for building Simple RESTful API using Flask and also to understand the concepts and techniques involved in API development.

#### Objectives:
1. Demonstrate how to create a basic Flask API.
2. Show how to define API endpoints and handle HTTP requests.
3. Illustrate how to return JSON responses.
4. Test the API using Postman.


![alt text](<img/fask API diagram.PNG>)



### OVERVIEW OF API (Application Programming Interface)

An API (Application Programming Interface) is a set of defined rules and protocols that enable different systems, services, or applications to communicate with each other and enabling data exchange and integration.
It acts like as :
1. Messenger: API acts as an intermediary, facilitating communication between systems.
2. Contract: API defines the rules and protocols for data exchange.
3. Gateway: API provides access to data, services, or functionality.

APIs enable:
1. Integration: Connecting different systems, services, or applications.
2. Data exchange: Sharing data between systems.
3. Microservices architecture: Breaking down complex systems into smaller, independent services.

APIs can be categorized as:
1. Server-side API: Hosted on a server, handling requests and returning responses.
2. Client-side API: Runs on the client-side (e.g., web browser), interacting with server-side APIs.

APIs can be hosted on a server, but they are not the server itself. Instead, they:
1. Expose endpoints: Specific URLs that handle requests.
2. Handle requests: Process incoming requests, validate data, and perform actions.
3. Return responses: Send data back to the requesting system.


Flask API:
A lightweight, flexible, and modular web framework for building web applications and APIs using Python.

"Flask" was named after the German word "Flasche," meaning "bottle," because it's small and lightweight, like a bottle.
and also contains a precious liquid (your application), just like a flask.

Armin Ronacher, Flask's creator, chose this name to convey the framework's Simplicity , flexibility and Portability

### How Do APIs Work?

### Basic Components of API
1)..**Request**: The client (e.g., your web browser) sends a request to the server. This request usually includes:
- Endpoint: The URL where the API can be accessed.
- Method: The type of action you want to perform (e.g., GET, POST, PUT, DELETE).
- Headers: Additional information sent with the request (e.g., authentication tokens).
- Body: The data sent with the request (for methods like POST or PUT).

2..**Response**: The server processes the request and sends back a response. This response typically includes:
- Status Code: Indicates the result of the request (e.g., 200 for success, 404 for not found).
- Headers: Additional information about the response.
- Body: The data returned by the server (usually in JSON format).

### Types of APIs
1)..**REST (Representational State Transfer)** 

- Most Common: REST APIs use standard HTTP methods (GET, POST, PUT, DELETE).

- Stateless: Each request from the client to the server must contain all the information needed to understand and process the request.

2)..**SOAP (Simple Object Access Protocol)**

- Older Protocol: Uses XML for message format and can be more complex.

- Highly Secure: Often used in enterprise-level applications where security and transaction management are crucial.
GraphQL

- Newer Query Language: Allows clients to request exactly the data they need.

- Flexible: Reduces the amount of data transferred over the network by allowing more specific queries.
Key Benefits of APIs

- Efficiency: APIs can be used to automate tasks, reducing the need for manual intervention.

- Innovation: APIs allow developers to access and integrate new technologies and services easily.

- Collaboration: Different teams can work on different parts of a system simultaneously, improving productivity and innovation.

---
### **Real life examples** 
Examples that illustrate how APIs enable interactions between different systems, services, or applications

#### Example 1: Social Media Sharing (Facebook API)
Built with: RESTful API, OAuth 2.0 authentication

Steps:
1. User clicks "Share" button on a website.
2. Website sends API request to Facebook API with user's access token.
3. Facebook API verifies access token and checks permissions.
4. Facebook API creates a new post on user's timeline.
5. Facebook API returns a response with post ID.

#### Example 2: E-commerce Product Search (Amazon Product Advertising API)
Built with: SOAP API, AWS Signature authentication

Steps:
1. User searches for products on an e-commerce website.
2. Website sends API request to Amazon Product Advertising API with search query.
3. Amazon API retrieves product data from database.
4. Amazon API returns response with product information in XML format.
5. Website displays product search results.


#### Example 3: Google Maps Integration (Google Maps API)
Built with: RESTful API, API key authentication

Steps:
1. User requests directions on a website.
2. Website sends API request to Google Maps API (`(link unavailable)) with API key.
3. Google Maps API retrieves directions data from database.
4. Google Maps API returns response with directions data in JSON format.
5. Website displays directions.
---

### PROJECT WORK FLOW

---
**TASK 1:** Create project folder in VScode ensure python is installed

- **Step-1** Set up project structure in VSCode using GitBash (Windows)

- **Step-2**: Open GitBash in vscode and navigate to your project directory (.vscode) and run command below to create project structure.

mkdir -p flask_api_project/{templates,static} && touch flask_api_project/app.py flask_api_project/templates/index.html flask_api_project/static/style.css && python -m venv flask_api_project/venv

- **Step-3**: Copy and paste applicable codes in app.py file, index.html file and style.css 


**TASK 2:** Run application in Windows (command prompt)

- **Step-1** : Run the commands below one by one

i) python -m venv venv

ii) venv/Scripts/activate  (change directory to activate file and press enter)

iv) pip install Flask  (change directory to scripts folder and run)

- **Step-2 :** Run flask application using the command below in flask_api directory

execute command *flask run*
Open your browser and go to http://127.0.0.1:5000 to see your application. 

**TASK 3:** Test the API with Postman
1. Download and install Postman
2. Open Postman and create a new request.
3. Set the request type to POST and enter http://127.0.0.1:5000/users. 
4. Click the "Send" button to send the request.
5. Verify the response in the Postman console.

### Key Concepts

1. Flask framework.
2. RESTful API design.
3. API endpoints.
4. HTTP requests and responses.
5. JSON serialization and deserialization.

---

### DCUMENTATION

----

### TASK-1 : API PROJECT FOLDER CREATION IN VScode 

- I Create project folder on desktop, I right clicked and opened in vscode.
I also ensured python is installed on my PC by going to microsoft store.


![alt text](<img/1 created project6 and opened with VSCODE.PNG>)


**Step-1**: The next things is I created API project directory structure and virtual environment by 

i) Open GitBash in vscode and cahnged directory to my project vscode folder (.vscode) and 

ii) I ran the command below to create project structure.

*mkdir -p flask_api_project/{templates,static} && touch flask_api_project/app.py flask_api_project/templates/index.html flask_api_project/static/style.css && python -m venv flask_api_project/venv*


![alt text](<img/2a create project directory structure _ virtual environ setup with mkdir command.PNG>)

This command creates a basic directory structure for a Flask API project and initializes essential files as explained below:

*mkdir -p flask_api_project/{templates,static}*

1. mkdir: Create a new directory.
2. -p: Create parent directories if they don't exist.
3. flask_api_project: The main project directory.
4. {templates,static}: Create subdirectories templates and static inside flask_api_project.


*touch flask_api_project/(link unavailable) flask_api_project/templates/index.html flask_api_project/static/style.css*

- touch: Create new empty files.
- flask_api_project/(link unavailable): The main Flask application file.
- flask_api_project/templates/index.html: An HTML template file.
- flask_api_project/static/style.css: A CSS stylesheet file.


*python -m venv flask_api_project/venv*

- python -m venv: Create a virtual environment.
- flask_api_project/venv: The virtual environment directory.


Resulting Directory Structure:

![alt text](<img/2b API project structure.PNG>)

![alt text](<img/2c flask project structure.PNG>)


Below is the breakdown of each component's function in the Flask API structure:


#### flask_api_project/


- Root directory of the Flask API project
- Contains all project files and subdirectories


#### templates/


- Directory for HTML templates
- Stores templates for rendering dynamic web pages
- Used by Flask's Jinja2 templating engine
- Functions:
    - Render HTML templates with dynamic data
    - Provide a user interface for API documentation or web application


#### index.html


- Main HTML template file
- Typically serves as the entry point for web applications
- Functions:
    - Display API documentation or web application UI
    - Handle user input and interactions


#### static/


- Directory for static assets
- Stores files that don't change frequently (e.g., images, CSS, JavaScript)
- Functions:
    - Serve static files to clients (e.g., web browsers)
    - Provide additional resources for web application or API documentation


#### style.css


- CSS stylesheet file
- Defines visual styling for HTML templates
- Functions:
    - Control layout, typography, and visual design
    - Enhance user experience and readability


#### venv/


- Virtual environment directory
- Isolated Python environment for the project
- Functions:
    - Manage project dependencies and packages
    - Provide a consistent and reproducible environment for development and deployment


In terms of interfacing or communicating with other systems:


- templates/ and static/: Serve web pages and static assets to clients (e.g., web browsers)
- venv/: Isolate project dependencies and ensure consistent environment for API interactions
- flask_api_project/: Host Flask API endpoints for interacting with other systems or services


Example interactions:

- Web browser requests index.html from templates/ and receives rendered HTML with dynamic data
- Web application requests static assets (e.g., images) from static/
- Other services or systems interact with Flask API endpoints hosted in flask_api_project/
- Flask API uses virtual environment venv/ to manage dependencies and ensure consistent behavior


This structure enables a Flask API to:

1. Serve web pages and static assets
2. Host API endpoints for interacting with other systems
3. Manage dependencies and ensure consistent environment
4. Provide a user interface for API documentation or web application





**Step-2**: I copied and pasted applicable codes in app.py file, index.html file and style.css inside vscode.

The codes below was copied and pasted in app.py file

from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

users = []

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/users', methods=['POST'])
def create_user():
    user = request.get_json()
    users.append(user)
    return jsonify(user), 201

@app.route('/users', methods=['GET'])
def get_users():
    return jsonify(users), 200

@app.route('/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    user = next((u for u in users if u['id'] == user_id), None)
    return jsonify(user), 200 if user else 404

@app.route('/users/<int:user_id>', methods=['PUT'])
def update_user(user_id):
    user = request.get_json()
    index = next((i for i, u in enumerate(users) if u['id'] == user_id), None)
    if index is not None:
        users[index] = user
        return jsonify(user), 200
    return '', 404

@app.route('/users/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    global users
    users = [u for u in users if u['id'] != user_id]
    return '', 204

if __name__ == '__main__':
    app.run(debug=True)



Implemented below in app.py

![alt text](<img/3 app.py code pasted.PNG>)


The codes below was copied and pasted in index.html file in the API template structure

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>API-Based Application</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>User Management</h1>
    <form id="userForm">
        <input type="text" id="name" placeholder="Name" required>
        <input type="email" id="email" placeholder="Email" required>
        <button type="submit">Add User</button>
    </form>
    <ul id="userList"></ul>

    <script>
        document.getElementById('userForm').addEventListener('submit', async function (event) {
            event.preventDefault();
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            
            const response = await fetch('/users', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ name, email })
            });

            const user = await response.json();
            document.getElementById('userList').innerHTML += `<li>${user.name} (${user.email})</li>`;
        });
    </script>
</body>
</html>

![alt text](<img/3 index.html codes.PNG>)


The codes below was copied and pasted in style.css file

body {
    font-family: Arial, sans-serif;
    margin: 20px;
}

form {
    margin-bottom: 20px;
}

input {
    margin-right: 10px;
}

![alt text](<img/3 style.css codes.PNG>)



### TASK-2 : RUN APPLICATION IN WINDOWS (command prompt)

- **Step-1** : Run the commands below one by one

i) python -m venv venv (change directory to PC-desktop)

ii) venv/Scripts/activate  (change directory to activate file and press enter)

iv) pip install Flask  (change directory to scripts folder and run)

![alt text](<img/4a Running pip install flask in activate directory.PNG>)

![alt text](<img/4b successful flask installation.PNG>)

**NOTE**

#### 1) *python -m venv venv* 
is a command used to create a virtual environment in Python.
Each part of the command means:

- python: Invokes the Python interpreter.
- -m: Stands for "module" and tells Python to run a module as a script.
- venv: Specifies the venv module, which is used for creating virtual environments.
- venv: The name of the virtual environment to be created.

After running the *python -m venv venv*, Python creates a new directory named venv containing the virtual environment.

Inside the venv directory, we have:

- or Scripts/ on Windows: Contains executable files (e.g., python, pip).
- Lib/ on Windows: Contains Python packages and libraries.
- include/: Contains header files for C/C++ extensions.

#### 2) *venv/Scripts/activate* (on Windows)

This command activates the virtual environment created earlier. When I  activated] the virtual environment:

- the command prompt/powershell indicates the virtual environment name (venv).
- my system's PATH environment variable is updated to point to the virtual environment's bin/Scripts directory.
- The virtual environment's Python interpreter and packages become the default.


**Activation** Isolates project dependencies from system-wide installations and Ensures reproducibility across environments.


#### 3) *pip install Flask* 
This command installs the Flask web framework using pip, the Python package installer.

During installation:
- pip searches for Flask on PyPI (Python Package Index).
- Downloads and installs Flask and its dependencies.
- Updates venv/Lib/site-packages with the new package.


**Flask installation** enables building web applications using Flask's lightweight framework and Provides access to Flask's extensive libraries and tools.



![alt text](<img/2c flask project structure.PNG>)


**Step-2 :** Run flask application using the command below in flask_api directory.

change directory to flask_api_project and execute command *flask run* as shown below

![alt text](<img/5 flask run in flask_api_project directory.PNG>)


**Step-3 :** Open your browser and go to http://127.0.0.1:5000 to see your application. 

![alt text](<img/6a open browser to see application.PNG>)

![alt text](<img/6b open browser to see application2.PNG>)


![alt text](<img/6c open browser to see application3.PNG>)


### TASK-3 : TEST API WITH POSTMAN
I followed the steps below to test my API with postman.

1. Firstly, I Downloaded and installed Postman

![alt text](<img/7 Postman download and installation.PNG>)


2. I Opened Postman and created a new colection.

![alt text](<img/7b Postman environ after login.PNG>)

![alt text](<img/7c create new collection.PNG>)


3. I created new request and set the request type to POST and enter http://127.0.0.1:5000/users. 

![alt text](<img/7d select POST.PNG>)

![alt text](<img/7e POST _ json format.PNG>)

4. I Clicked the "Send" button to send the request and Verified the response in the Postman console.

![alt text](<img/7f OUTPUT.PNG>)

The END