# Weather-app
Frontend app
npx create-react-app weather-app
cd weather-app
npm start
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [city, setCity] = useState("");
  const [weather, setWeather] = useState(null);
  const [error, setError] = useState("");

  const apiKey = "YOUR_API_KEY_HERE"; // Replace with your OpenWeatherMap API key

  const getWeather = async () => {
    if (!city) {
      setError("Please enter a city name");
      return;
    }

    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`
      );
      const data = await response.json();

      if (data.cod === "404") {
        setError("City not found");
        setWeather(null);
      } else {
        setWeather(data);
        setError("");
      }
    } catch (err) {
      setError("Error fetching weather data");
    }
  };

  return (
    <div className="app">
      <h1>🌦️ Weather App</h1>

      <div className="search">
        <input
          type="text"
          placeholder="Enter city name..."
          value={city}
          onChange={(e) => setCity(e.target.value)}
        />
        <button onClick={getWeather}>Search</button>
      </div>

      {error && <p className="error">{error}</p>}

      {weather && (
        <div className="weather-info">
          <h2>
            {weather.name}, {weather.sys.country}
          </h2>
          <p>🌡️ Temperature: {weather.main.temp}°C</p>
          <p>☁️ Condition: {weather.weather[0].description}</p>
          <p>💧 Humidity: {weather.main.humidity}%</p>
          <p>🌬️ Wind Speed: {weather.wind.speed} m/s</p>
        </div>
      )}
    </div>
  );
};

export default App;
body {
  background: linear-gradient(to right, #74ebd5, #acb6e5);
  font-family: "Poppins", sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.app {
  background: white;
  padding: 30px;
  border-radius: 20px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
  text-align: center;
  width: 350px;
}

.search {
  margin-bottom: 20px;
}

.search input {
  padding: 10px;
  width: 70%;
  border: 2px solid #ccc;
  border-radius: 8px;
  outline: none;
}

.search button {
  padding: 10px 15px;
  margin-left: 10px;
  background: #4b6cb7;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.3s;
}

.search button:hover {
  background: #182848;
}

.weather-info {
  margin-top: 15px;
}

.error {
  color: red;
  font-weight: bold;
}

