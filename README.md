<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Weather Dashboard</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
  <style>
    body {
      background: #f0f4f8;
      font-family: Arial, sans-serif;
    }
    .weather-container {
      max-width: 600px;
      margin: 60px auto;
      background: white;
      padding: 25px;
      border-radius: 10px;
      box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
    }
    .card {
      margin-top: 20px;
    }
    .error {
      color: red;
      margin-top: 15px;
    }
  </style>
</head>
<body>

<div class="container weather-container">
  <h2 class="text-center mb-4">🌦️ Weather Dashboard</h2>
  
  <div class="input-group">
    <input type="text" id="cityInput" class="form-control" placeholder="Enter city name (e.g., Rampur,IN)" />
    <button class="btn btn-primary" onclick="getWeather()">Get Weather</button>
  </div>

  <div id="error" class="error d-none"></div>

  <div id="weatherCard" class="card d-none">
    <div class="card-body text-center">
      <h4 id="cityName" class="card-title"></h4>
      <p id="weatherDesc" class="card-text"></p>
      <h5><span id="temp"></span>°C</h5>
      <p>Humidity: <span id="humidity"></span>%</p>
      <p>Wind Speed: <span id="wind"></span> km/h</p>
    </div>
  </div>
</div>

<script>
  async function getWeather() {
    const apiKey = '199ef687e5fd367deccd2249d391982f'; 
    const city = document.getElementById('cityInput').value.trim();
    const weatherCard = document.getElementById('weatherCard');
    const errorDiv = document.getElementById('error');

    if (!city) {
      showError('Please enter a city name.');
      return;
    }

    try {
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;
      const response = await fetch(url);

      if (!response.ok) {
        throw new Error('City not found or invalid input');
      }

      const data = await response.json();

      // Fill in the weather details
      document.getElementById('cityName').innerText = data.name + ", " + data.sys.country;
      document.getElementById('weatherDesc').innerText = data.weather[0].description;
      document.getElementById('temp').innerText = Math.round(data.main.temp);
      document.getElementById('humidity').innerText = data.main.humidity;
      document.getElementById('wind').innerText = data.wind.speed;

      weatherCard.classList.remove('d-none');
      errorDiv.classList.add('d-none');
    } catch (error) {
      weatherCard.classList.add('d-none');
      showError("Error: Could not fetch weather data. Please check the city name.");
    }
  }

  function showError(message) {
    const errorDiv = document.getElementById('error');
    errorDiv.innerText = message;
    errorDiv.classList.remove('d-none');
  }
</script>

</body>
</html>
