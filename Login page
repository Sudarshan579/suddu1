 <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title> Login</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      background: linear-gradient(to right, #667eea, #764ba2);
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .login-card {
      background-color: white;
      border-radius: 15px;
      box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
      padding: 40px;
      width: 100%;
      max-width: 400px;
      transition: transform 0.3s ease;
    }

    .login-card:hover {
      transform: translateY(-5px);
    }

    .login-card h2 {
      text-align: center;
      margin-bottom: 30px;
      color: #333;
    }

    .form-group {
      margin-bottom: 25px;
      position: relative;
    }

    .form-group input {
      width: 100%;
      padding: 12px 15px;
      border: 2px solid #ddd;
      border-radius: 8px;
      font-size: 16px;
      transition: border-color 0.3s;
    }

    .form-group input:focus {
      border-color: #667eea;
      outline: none;
    }

    .form-group label {
      position: absolute;
      top: 50%;
      left: 15px;
      transform: translateY(-50%);
      background-color: white;
      padding: 0 5px;
      color: #999;
      transition: 0.3s;
      pointer-events: none;
    }

    .form-group input:focus + label,
    .form-group input:not(:placeholder-shown) + label {
      top: -10px;
      left: 10px;
      font-size: 12px;
      color: #667eea;
    }

    .login-btn {
      width: 100%;
      padding: 12px;
      background: #667eea;
      color: white;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
      transition: background 0.3s;
    }

    .login-btn:hover {
      background: #5a67d8;
    }

    .link {
      text-align: center;
      margin-top: 15px;
    }

    .link a {
      text-decoration: none;
      color: #667eea;
      font-size: 14px;
      transition: color 0.3s;
    }

    .link a:hover {
      color: #5a67d8;
    }

    @media (max-width: 480px) {
      .login-card {
        padding: 30px 20px;
      }
    }
  </style>
</head>
<body>

  <div class="login-card">
    <h2>Student Login</h2>
    <form onsubmit="return handleLogin(event)">
      <div class="form-group">
        <input type="email" id="email" placeholder=" " required />
        <label for="email">Email Address</label>
      </div>
      <div class="form-group">
        <input type="password" id="password" placeholder=" " required />
        <label for="password">Password</label>
      </div>
      <button type="submit" class="login-btn">Login</button>
    </form>
    <div class="link">
      <a href="#">Forgot Password?</a>
    </div>
  </div>

  <script>
    function handleLogin(event) {
      event.preventDefault();
      const email = document.getElementById("email").value.trim();
      const password = document.getElementById("password").value.trim();

      const passwordPattern = /^(?=.*[0-9])(?=.*[!@#$%^&*]).{8,}$/;
      if (!passwordPattern.test(password)) {
        alert("Password must be at least 8 characters long and include a number and special symbol.");
        return false;
      }

      // Simulate login success
      alert("Login successful!");
      window.location.href = "suhome.html";
    }
  </script>
</body>
</html>
