---
title: Account Login
layout: base
---
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Account Login</title>
<style>
        .normal {
            background-color: #121212 !important;
            color: white !important;
        }
        .lightmode {
            background-color: #F6FFF5 !important;
            color: black !important;
        }
        .post-title {
            text-align: center;
            font-size: 3em;
            font-weight: bold;
            margin: 0;
            padding: 0;
            width: 40%;
            margin-left: 30%;
        }
        body {
            font-family: Arial, sans-serif;
            background-color: #F2F2F2;
            margin: 0;
            display: flex;
            justify-content: center;
        }
        .container {
            background-color: white;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
            text-align: center;
        }
        .button {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #007BFF;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .normal {
            background-color: #121212;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            margin: 5px;
        }
        .normal input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid white;
        }
        .normal button {
            background-color: #121212;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            width: 50%;
        }
        .normal button:hover {
            background-color: #333;
        }
        .lightmode {
            background-color: #F6FFF5;
            color: black;
        }
        #logind {
            display: flex;
            justify-content: center;
            width: 60%;
            margin-left: 20%;
        }
        #signUpButton {
            display: flex;
            justify-content: center;
            width: 20%;
            margin: 10px 0px 0px;
            margin-left: 40%;
            background-color: light blue;
        }
            ::-webkit-scrollbar {
        width: 8px;
    }
    ::-webkit-scrollbar-track {
        background: #333;
    }
    ::-webkit-scrollbar-thumb {
        background: #a0daf2;
        border-radius: 4px;
        width: 4px; 
    }
    ::-webkit-scrollbar-thumb:hover {
        background: #FFF;
        width: 4px;
    }
    </style>
</head>
<body>
    <div class="container">
        <!-- Login Screen -->
        <div id="loginScreen">
            <form action="javascript:login_user()">
                <p id="email" class="normal">
                    <label>Email:
                        <input class="normal" type="text" name="uid" id="uid" required>
                    </label>
                </p>
                <p id="passwordd" class="normal">
                    <label>Password:
                        <input class="normal" type="password" name="password" id="password" required>
                    </label>
                </p>
                <p id="logind" class="normal">
                    <button>Login</button>
                </p>
            </form>
        </div>
        <!-- Account Details Screen (Initially Hidden) -->
        <div id="accountDetails" style="display: none;">
            <!-- Account details will go here -->
            <p>Welcome to your account!</p>
        </div>
    </div>
    <button id="signUpButton" class="button" onclick="signUpSwitch()">Sign Up</button>
</body>
<script>
        document.addEventListener("DOMContentLoaded", function () {
            // Check if the user is logged in (You need to define your own logic for this)
            if (localStorage.getItem("loggedIn") === "true") {
                showAccountDetails();
            } else {
                showLoginScreen();
            }
        });
        function showAccountDetails() {
            document.getElementById("loginScreen").style.display = "none";
            document.getElementById("signUpButton").style.display = "none";
            document.getElementById("accountDetails").style.display = "block";
            // Create and append the email and stock elements
            const emailDiv = document.createElement("div");
            emailDiv.innerHTML = "Email: " + localStorage.getItem("localEmail");
            document.getElementById("accountDetails").appendChild(emailDiv);
            const stockDiv = document.createElement("div");
            document.getElementById("accountDetails").appendChild(stockDiv);
            // Create a button element
            const button = document.createElement('button');
            button.innerText = 'LOG OUT';
            button.addEventListener('click', () => {
                // Remove the loggedIn flag from localStorage
                localStorage.removeItem("localEmail");
                localStorage.removeItem("localPassword");
                localStorage.removeItem("loggedIn");
                // Show the login screen
                showLoginScreen();
            });
            // Append the logout button to the account details section
            document.getElementById("accountDetails").appendChild(button);
        }
        function showLoginScreen() {
            document.getElementById("loginScreen").style.display = "block";
            document.getElementById("signUpButton").style.display = "block";
            document.getElementById("accountDetails").style.display = "none";
        }
        function signUpSwitch() {
            window.location.href = "/Studious-Scrummers-Frontend/signup";
        }
        function login_user() {
            // You can make a POST request here to your authentication endpoint
            var url = "http://localhost:8999";
            // Comment out next line for local testing
            //  url = "https://studious.duckdns.org"; 
            const login_url = url + '/authenticate';
            const body = {
                email: document.getElementById("uid").value,
                password: document.getElementById("password").value,
            };
            console.log(JSON.stringify(body));
            const requestOptions = {
                method: 'POST',
                mode: 'cors',
                cache: 'no-cache',
                credentials: 'include',
                body: JSON.stringify(body),
                headers: {
                    "content-type": "application/json",
                    "Access-Control-Allow-Credentials": "true",
                    "Access-Control-Allow-Origin": "*",
                },
            };
            // Fetch JWT
            fetch(login_url, requestOptions)
                .then(response => {
                    if (!response.ok) {
                        const errorMsg = 'Login error: ' + response.status;
                        console.log(errorMsg);
                        return;
                    }
                    // Success!!!
                    // Redirect to Database location
                    localStorage.setItem("localEmail", document.getElementById("uid").value);
                    localStorage.setItem("localPassword", document.getElementById("password").value);
                    console.log(localStorage.getItem("localEmail"));
                    console.log(localStorage.getItem("localPassword"));
                    localStorage.setItem("loggedIn", "true");
                    showAccountDetails();
                    window.location.href = "/sturdy-fiesta/login";
                });
        }
    </script>