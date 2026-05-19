# Ex06 BMI Calculator
## Date: 19.05.2026

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
### Home.jsx
```
import React from "react";
import { useNavigate } from "react-router-dom";

const Home = () => {
  const navigate = useNavigate();

  return (
    <div style={styles.container}>
      <h1 style={styles.title}>Welcome to BMI Calculator</h1>
      <p style={styles.text}>Check your Body Mass Index (BMI) easily!</p>
      <button onClick={() => navigate("/bmi")} style={styles.button}>
        Go to Calculator
      </button>
    </div>
  );
};

const styles = {
  container: {
    height: "100vh",
    display: "flex",
    flexDirection: "column",
    alignItems: "center",
    justifyContent: "center",
    backgroundColor: "#e3f2fd",
  },
  title: {
    fontSize: "2.5rem",
    marginBottom: "1rem",
  },
  text: {
    fontSize: "1.2rem",
    marginBottom: "2rem",
  },
  button: {
    padding: "10px 20px",
    backgroundColor: "#1976d2",
    color: "white",
    border: "none",
    borderRadius: "8px",
    cursor: "pointer",
    fontSize: "1rem",
  },
};

export default Home;
```
### BMICalculator.jsx
```
import React, { useState } from "react";
import { useNavigate } from "react-router-dom";

const BMICalculator = () => {
  const [height, setHeight] = useState("");
  const [weight, setWeight] = useState("");
  const [error, setError] = useState("");
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();

    if (!height || !weight || height <= 0 || weight <= 0) {
      setError("Please enter valid height and weight!");
      return;
    }

    setError("");
    navigate(`/result?height=${height}&weight=${weight}`);
  };

  return (
    <div style={styles.container}>
      <h2 style={styles.title}>BMI Calculator</h2>
      <form onSubmit={handleSubmit} style={styles.form}>
        <label style={styles.label}>Height (in cm):</label>
        <input
          type="number"
          value={height}
          onChange={(e) => setHeight(e.target.value)}
          style={styles.input}
          required
        />

        <label style={styles.label}>Weight (in kg):</label>
        <input
          type="number"
          value={weight}
          onChange={(e) => setWeight(e.target.value)}
          style={styles.input}
          required
        />

        {error && <p style={styles.error}>{error}</p>}

        <button type="submit" style={styles.button}>
          Calculate BMI
        </button>
      </form>
    </div>
  );
};

const styles = {
  container: {
    height: "100vh",
    display: "flex",
    flexDirection: "column",
    alignItems: "center",
    justifyContent: "center",
    backgroundColor: "#fff3e0",
  },
  title: { fontSize: "2rem", marginBottom: "1.5rem" },
  form: {
    display: "flex",
    flexDirection: "column",
    gap: "10px",
    width: "300px",
  },
  label: { fontSize: "1rem" },
  input: {
    padding: "10px",
    fontSize: "1rem",
    borderRadius: "8px",
    border: "1px solid #ccc",
  },
  button: {
    padding: "10px",
    backgroundColor: "#ff9800",
    color: "white",
    border: "none",
    borderRadius: "8px",
    cursor: "pointer",
    marginTop: "10px",
  },
  error: { color: "red", fontSize: "0.9rem" },
};

export default BMICalculator;
```
### Result.jsx
```
import React from "react";
import { useLocation, useNavigate } from "react-router-dom";

const Result = () => {
  const location = useLocation();
  const navigate = useNavigate();

  const queryParams = new URLSearchParams(location.search);
  const height = parseFloat(queryParams.get("height"));
  const weight = parseFloat(queryParams.get("weight"));

  const heightInMeters = height / 100;
  const bmi = (weight / (heightInMeters * heightInMeters)).toFixed(2);

  let category = "";
  if (bmi < 18.5) category = "Underweight";
  else if (bmi >= 18.5 && bmi < 24.9) category = "Normal";
  else if (bmi >= 25 && bmi < 29.9) category = "Overweight";
  else category = "Obese";

  return (
    <div style={styles.container}>
      <h2 style={styles.title}>Your BMI Result</h2>
      <p style={styles.text}>Height: {height} cm</p>
      <p style={styles.text}>Weight: {weight} kg</p>
      <h3 style={styles.result}>Your BMI: {bmi}</h3>
      <h3 style={styles.category}>Category: {category}</h3>

      <button onClick={() => navigate("/bmi")} style={styles.button}>
        Calculate Again
      </button>
    </div>
  );
};

const styles = {
  container: {
    height: "100vh",
    display: "flex",
    flexDirection: "column",
    alignItems: "center",
    justifyContent: "center",
    backgroundColor: "#f1f8e9",
  },
  title: { fontSize: "2rem", marginBottom: "1rem" },
  text: { fontSize: "1.1rem" },
  result: { fontSize: "1.5rem", color: "#2e7d32" },
  category: { fontSize: "1.3rem", color: "#1b5e20", marginBottom: "1.5rem" },
  button: {
    padding: "10px 20px",
    backgroundColor: "#4caf50",
    color: "white",
    border: "none",
    borderRadius: "8px",
    cursor: "pointer",
  },
};

export default Result;
```
### App.jsx

```
import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./components/Home";
import BMICalculator from "./components/BMICalculator";
import Result from "./components/Result";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/bmi" element={<BMICalculator />} />
        <Route path="/result" element={<Result />} />
      </Routes>
    </Router>
  );
}

export default App;
```


## OUTPUT
<img width="1917" height="1142" alt="image" src="https://github.com/user-attachments/assets/9446ff6b-341b-48dd-983d-fcac7cc84c6f" />

<img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/ef2a6a9c-6c1b-4fe1-8a12-cc70141590d4" />

<img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/a87dad0b-2f9d-4351-a6e9-863ab23b0778" />

## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
