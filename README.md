<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login & Register System</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        .container {
            background-color: white;
            padding: 40px;
            border-radius: 10px;
            box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.1);
            width: 300px;
        }
        h2 {
            text-align: center;
            margin-bottom: 30px;
        }
        input {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .error {
            color: red;
            text-align: center;
        }
        .info-bar {
            background-color: #ffcc00;
            padding: 10px;
            margin-bottom: 20px;
            font-weight: bold;
            border-radius: 5px;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            display: inline-block;
            text-align: left;
        }
    </style>
</head>
<body>

    <!-- Registration Form -->
    <div class="container" id="registerForm">
        <h2>Register</h2>
        <form id="registerFormElement">
            <input type="text" id="regUsername" placeholder="Enter Username" required>
            <input type="password" id="regPassword" placeholder="Enter Password" required>
            <button type="submit">Register</button>
        </form>
        <div id="registerError" class="error"></div>
        <p>Already have an account? <a href="javascript:void(0);" onclick="showLoginForm()">Login</a></p>
    </div>

    <!-- Login Form -->
    <div class="container" id="loginForm" style="display: none;">
        <h2>Login</h2>
        <form id="loginFormElement">
            <input type="text" id="username" placeholder="Enter Username" required>
            <input type="password" id="password" placeholder="Enter Password" required>
            <button type="submit">Login</button>
        </form>
        <div id="loginError" class="error"></div>
        <p>Don't have an account? <a href="javascript:void(0);" onclick="showRegisterForm()">Register</a></p>
    </div>

    <!-- IP Location Finder -->
    <div class="container" id="ipLocationForm" style="display: none;">
        <h2>IP Address Location Finder</h2>
        <div class="info-bar">
            <p>⚠️ **Warning:** This tool is for authorized use only. Ensure proper authorization before tracking IP addresses. Use responsibly and in accordance with the law.</p>
        </div>
        <input type="text" id="ipInput" placeholder="Enter IP Address (or leave empty to find your own)">
        <button onclick="getIPDetails()">Search</button>
        <div id="result"></div>
    </div>

    <script>
        let users = [];

        // Show Register Form
        function showRegisterForm() {
            document.getElementById("registerForm").style.display = "block";
            document.getElementById("loginForm").style.display = "none";
            document.getElementById("ipLocationForm").style.display = "none";
        }

        // Show Login Form
        function showLoginForm() {
            document.getElementById("registerForm").style.display = "none";
            document.getElementById("loginForm").style.display = "block";
            document.getElementById("ipLocationForm").style.display = "none";
        }

        // Register Function
        document.getElementById("registerFormElement").addEventListener("submit", function(e) {
            e.preventDefault(); // Prevent form from submitting

            let regUsername = document.getElementById("regUsername").value;
            let regPassword = document.getElementById("regPassword").value;

            // Simple check to see if username already exists
            if (users.some(user => user.username === regUsername)) {
                document.getElementById("registerError").innerHTML = "Username already exists!";
            } else {
                users.push({ username: regUsername, password: regPassword });
                alert("Registration successful! Please login.");
                showLoginForm(); // Show login form after successful registration
            }
        });

        // Login Function
        document.getElementById("loginFormElement").addEventListener("submit", function(e) {
            e.preventDefault(); // Prevent form from submitting

            let username = document.getElementById("username").value;
            let password = document.getElementById("password").value;

            // Simple login check
            let user = users.find(user => user.username === username && user.password === password);
            if (user) {
                alert("Login successful!");
                document.getElementById("loginForm").style.display = "none";
                document.getElementById("ipLocationForm").style.display = "block";  // Show IP Location Finder
            } else {
                document.getElementById("loginError").innerHTML = "Invalid username or password!";
            }
        });

        // IP Location Finder Function
        function getIPDetails() {
            let ip = document.getElementById("ipInput").value;
            let apiUrl = ip ? `https://ipinfo.io/${ip}/json?token=60ea39d1a09c78` : `https://ipinfo.io/json?token=60ea39d1a09c78`;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    if (data.error) {
                        document.getElementById("result").innerHTML = "Invalid IP Address!";
                    } else {
                        let loc = data.loc.split(",");
                        document.getElementById("result").innerHTML = `
                            <strong>IP Address:</strong> ${data.ip} <br>
                            <strong>Country:</strong> ${data.country} <br>
                            <strong>Region:</strong> ${data.region} <br>
                            <strong>City:</strong> ${data.city} <br>
                            <strong>ISP:</strong> ${data.org} <br>
                            <strong>Latitude:</strong> ${loc[0]} <br>
                            <strong>Longitude:</strong> ${loc[1]} <br>
                            <a href="https://www.google.com/maps?q=${loc[0]},${loc[1]}" target="_blank">View on Google Maps</a>
                            <br><br>
                            <iframe src="https://maps.google.com/maps?q=${loc[0]},${loc[1]}&output=embed"></iframe>
                        `;
                    }
                })
                .catch(error => {
                    document.getElementById("result").innerHTML = "Error fetching data!";
                });
        }

        // Load the register form initially
        showRegisterForm();
    </script>

</body>
</html>.
