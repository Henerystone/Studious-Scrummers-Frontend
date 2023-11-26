<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Account Sign Up</title>
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
            background-color: #f2f2f2;
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
        #logInButton {
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
            <form action="javascript:signUpRequest()">
                <p id="name" class="normal">
                    <label>Name:
                        <input class="normal" type="text" name="legalName" id="legalName" required>
                    </label>
                </p>
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
                    <button>Sign Up</button>
                </p>
            </form>
        </div>
        <!-- Account Details Screen (Initially Hidden) -->
        <div id="accountDetails" style="display: none;">
            <!-- Account details will go here -->
            <p>Welcome to your account. Your account details are displayed here.</p>
        </div>
    </div>
    <button id="logInButton" class="button" onclick="logInSwitch()">Log In</button>
</body>

<script>
    function logInSwitch() {
        window.location.href = "/sturdy-fiesta/login";
    }

    function signUpUser() {
        console.log("signUpUser() called");
        var url = "http://localhost:8765";

            // Comment out next line for local testing
            // url = "https://no-papels.stu.nighthawkcodingsociety.com";
            const login_url = url + '/api/person/post'; 
            const body = {
                name: document.getElementById("legalName").value,
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
                    console.log("success")
                    window.location.href = "/sturdy-fiesta/login";
                });
    }

    function signUpRequest() {
        var requestOptions = {
            method: 'POST',
            mode: 'cors',
            cache: 'no-cache',
            credentials: 'include',
        };

        let fetchName = document.getElementById("legalName").value
        let fetchEmail = document.getElementById("uid").value
        let fetchPassword = document.getElementById("password").value

        fetch(`http://localhost:8765/api/person/post?email=${fetchEmail}&password=${fetchPassword}@123&name=${fetchName}`, requestOptions)
        .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                return;
            }
            // Success!!!
            // Redirect to Database location
            console.log("success")
            window.location.href = "/sturdy-fiesta/login";
        });
    }
</script>