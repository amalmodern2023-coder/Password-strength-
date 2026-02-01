<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Password Strength Meter</title>
  <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    .container { max-width: 400px; margin: auto; }
    input { padding: 8px; width: 100%; margin-bottom: 10px; }
    .strength {
      height: 10px;
      width: 100%;
      background-color: #ddd;
      border-radius: 5px;
      overflow: hidden;
      margin-bottom: 10px;
    }
    .strength-bar {
      height: 100%;
      width: 0%;
      transition: width 0.3s;
    }
    .weak { background-color: red; }
    .medium { background-color: orange; }
    .strong { background-color: green; }
  </style>
</head>
<body>
  <div id="root"></div>

  <script type="text/babel">

    const PasswordStrength = () => {
      const [password, setPassword] = React.useState("");

      // حساب قوة الباسورد
      const calculateStrength = (pass) => {
        let strength = 0;
        if (pass.length >= 6) strength++;
        if (/[A-Z]/.test(pass)) strength++;
        if (/[0-9]/.test(pass)) strength++;
        if (/[^A-Za-z0-9]/.test(pass)) strength++;
        return strength;
      };

      const strength = calculateStrength(password);

      const getStrengthClass = () => {
        if (strength <= 1) return "weak";
        if (strength <= 3) return "medium";
        return "strong";
      };

      const getStrengthWidth = () => (strength / 4) * 100 + "%";

      const getStrengthLabel = () => {
        if (strength <= 1) return "Weak";
        if (strength <= 3) return "Medium";
        return "Strong";
      };

      return (
        <div className="container">
          <h2>Password Strength Meter</h2>
          <input
            type="password"
            placeholder="Enter password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
          />
          <div className="strength">
            <div
              className={`strength-bar ${getStrengthClass()}`}
              style={{ width: getStrengthWidth() }}
            ></div>
          </div>
          <p>Password Strength: {getStrengthLabel()}</p>
        </div>
      );
    };

    const root = ReactDOM.createRoot(document.getElementById("root"));
    root.render(<PasswordStrength />);

  </script>
</body>
</html>
