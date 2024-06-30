<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Password Strength Tester</title>
<style>
  body {
    font-family: Arial, sans-serif;
    margin: 20px;
  }
  .password-strength {
    margin-top: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  .suggestion {
    margin-top: 10px;
    color: #FF6347;
    font-style: italic;
  }
</style>
</head>
<body>
  <h2>Password Strength Tester</h2>
  <label for="password">Enter your password:</label>
  <input type="password" id="password" oninput="checkPasswordStrength()">
  
  <div class="password-strength" id="password-strength">
    <p>Password strength: <span id="strength-text"></span></p>
    <p class="suggestion" id="password-suggestion"></p>
  </div>

  <script>
    function checkPasswordStrength() {
      var password = document.getElementById("password").value;
      var strengthText = document.getElementById("strength-text");
      var suggestionText = document.getElementById("password-suggestion");

      // Reset strength and suggestion texts
      strengthText.textContent = "";
      suggestionText.textContent = "";

      // Check length
      var strength = checkLength(password);

      // Check character diversity
      strength += checkCharacterDiversity(password);

      // Check entropy (complexity)
      strength += checkEntropy(password);

      // Check vulnerability to common attacks
      var vulnerability = checkCommonAttacks(password);

      // Update strength text based on total score
      if (strength >= 16) {
        strengthText.textContent = "Excellent";
      } else if (strength >= 12) {
        strengthText.textContent = "Very Strong";
      } else if (strength >= 8) {
        strengthText.textContent = "Strong";
      } else if (strength >= 5) {
        strengthText.textContent = "Moderate";
      } else {
        strengthText.textContent = "Weak";
      }

      // Provide suggestions based on vulnerability
      if (vulnerability.length > 0) {
        suggestionText.textContent = "Consider avoiding common patterns or words like: " + vulnerability.join(", ");
      }
    }

    function checkLength(password) {
      if (password.length >= 12) {
        return 5;
      } else if (password.length >= 8) {
        return 3;
      } else {
        return 0;
      }
    }

    function checkCharacterDiversity(password) {
      var categories = 0;
      if (/[A-Z]/.test(password)) categories++; // Uppercase letters
      if (/[a-z]/.test(password)) categories++; // Lowercase letters
      if (/\d/.test(password)) categories++;    // Numbers
      if (/[^\w\d]/.test(password)) categories++; // Special characters

      return categories;
    }

    function checkEntropy(password) {
      // Simple entropy calculation (for demonstration purposes)
      var entropy = 0;
      var charset = 0;
      if (/[A-Z]/.test(password)) charset += 26; // Uppercase letters
      if (/[a-z]/.test(password)) charset += 26; // Lowercase letters
      if (/\d/.test(password)) charset += 10;    // Numbers
      if (/[^\w\d]/.test(password)) charset += 30; // Special characters

      entropy = Math.log2(Math.pow(charset, password.length));
      return Math.floor(entropy / 10); // Adjust scale to match other criteria
    }

    function checkCommonAttacks(password) {
      var commonAttacks = ["password", "123456", "qwerty", "letmein", "monkey"]; // Example common passwords
      var foundAttacks = [];

      commonAttacks.forEach(function(attack) {
        if (password.toLowerCase().includes(attack)) {
          foundAttacks.push(attack);
        }
      });

      return foundAttacks;
    }
  </script>
</body>
</html>
