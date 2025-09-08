In this project i am working with the Front-end technologies such as HTML,CSS, and JavaScript - For Fatching real time climate information i have to 
use Weather API the rrequest the actual Server and provides real original CLimate information city by city.
Here is my Code Files Bellow.
-
-
-

For Demo CLick Here : https://peaceful-daifuku-ab79e2.netlify.app/


#HTML : 
________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App</title>
  <link rel="stylesheet" href="style.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
</head>
<body>
  <div class="container">
    <h1>Weather App</h1>
    <div class="search-box">
      <input type="text" id="city-input" placeholder="Enter city name">
      <button id="search-btn">Search</button>
    </div>
    <div id="loading" class="loading">Loading...</div>
    <div id="weather-result" class="weather-result">
      <!-- Weather data will be displayed here -->
    </div>
    <div class="social-media">
      <a href="https://www.linkedin.com/in/rahul-sonawane-32270031a?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app" target="_blank" class="social-icon">
        <i class="fab fa-linkedin"></i>
      </a>
      <a href="https://www.instagram.com/rahulsonawane536?igsh=NWthcDl4ZXIxZ3Yz" target="_blank" class="social-icon">
        <i class="fab fa-instagram"></i>
      </a>
    </div>
    <footer>
      <p>Created by Rahul Sonawane</p>
    </footer>
  </div>
  <script src="script.js"></script>
</body>
</html>
________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

#CSS : 
body {
    font-family: 'Arial', sans-serif;
    background: linear-gradient(135deg, #1e3c72, #2a5298);
    color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
  }
  
  .container {
    background: rgba(255, 255, 255, 0.1);
    padding: 20px 30px;
    border-radius: 15px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
    text-align: center;
    backdrop-filter: blur(10px);
  }
  
  h1 {
    margin-bottom: 20px;
    font-size: 2.5em;
  }
  
  .search-box {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
  }
  
  input {
    padding: 10px;
    width: 250px;
    border: none;
    border-radius: 5px 0 0 5px;
    outline: none;
  }
  
  button {
    padding: 10px 20px;
    background: #007bff;
    color: #fff;
    border: none;
    border-radius: 0 5px 5px 0;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  
  button:hover {
    background: #0056b3;
  }
  
  .loading {
    display: none;
    font-size: 1.2em;
    margin-top: 20px;
  }
  
  .weather-result {
    margin-top: 20px;
  }
  
  .weather-result h2 {
    font-size: 2em;
    margin-bottom: 10px;
  }
  
  .weather-result p {
    font-size: 1.2em;
    margin: 5px 0;
  }
  
  .weather-result img {
    width: 100px;
    height: 100px;
    margin-top: 10px;
  }
  
  .error {
    color: #ff6b6b;
    font-size: 1.2em;
    margin-top: 20px;
  }
  
  .social-media {
    margin-top: 30px;
    display: flex;
    justify-content: center;
    gap: 20px;
  }
  
  .social-icon {
    color: #fff;
    font-size: 24px;
    transition: color 0.3s ease, transform 0.3s ease;
  }
  
  .social-icon:hover {
    color: #007bff;
    transform: scale(1.2);
  }
  
  footer {
    margin-top: 30px;
    font-size: 1em;
    color: #fff;
    opacity: 0.8;
    text-align: center;
  }
  
  footer p {
    margin: 0;
  }

  ______________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

  JavaScript : 

  const apiKey = '8bd439bdf83cc8eef278edd2cf48992b'; // Replace with your OpenWeatherMap API key

document.getElementById('search-btn').addEventListener('click', () => {
  const city = document.getElementById('city-input').value;
  if (city) {
    fetchWeather(city);
  } else {
    showError('Please enter a city name.');
  }
});

async function fetchWeather(city) {
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;
  showLoading();

  try {
    const response = await fetch(url);
    const data = await response.json();

    if (data.cod === 200) {
      displayWeather(data);
    } else {
      showError('City not found. Please try again.');
    }
  } catch (error) {
    showError('An error occurred. Please try again later.');
  } finally {
    hideLoading();
  }
}

function displayWeather(data) {
  const weatherResult = document.getElementById('weather-result');
  const cityName = data.name;
  const temperature = data.main.temp;
  const weatherDescription = data.weather[0].description;
  const icon = data.weather[0].icon;
  const humidity = data.main.humidity;
  const windSpeed = data.wind.speed;
  const pressure = data.main.pressure;

  weatherResult.innerHTML = `
    <h2>${cityName}</h2>
    <p>Temperature: ${temperature}°C</p>
    <p>Weather: ${weatherDescription}</p>
    <img src="http://openweathermap.org/img/wn/${icon}@2x.png" alt="${weatherDescription}">
    <p>Humidity: ${humidity}%</p>
    <p>Wind Speed: ${windSpeed} m/s</p>
    <p>Pressure: ${pressure} hPa</p>
  `;

  // Change background based on weather condition
  changeBackground(weatherDescription);
}

function showLoading() {
  document.getElementById('loading').style.display = 'block';
  document.getElementById('weather-result').innerHTML = '';
}

function hideLoading() {
  document.getElementById('loading').style.display = 'none';
}

function showError(message) {
  const weatherResult = document.getElementById('weather-result');
  weatherResult.innerHTML = `<div class="error">${message}</div>`;
}

function changeBackground(weatherDescription) {
  const body = document.body;
  if (weatherDescription.includes('rain')) {
    body.style.background = 'linear-gradient(135deg, #4b6cb7, #182848)';
  } else if (weatherDescription.includes('cloud')) {
    body.style.background = 'linear-gradient(135deg, #6a85b6, #bac8e0)';
  } else if (weatherDescription.includes('clear')) {
    body.style.background = 'linear-gradient(135deg, #56ccf2, #2f80ed)';
  } else if (weatherDescription.includes('snow')) {
    body.style.background = 'linear-gradient(135deg, #e0e0e0, #f5f5f5)';
  } else {
    body.style.background = 'linear-gradient(135deg, #1e3c72, #2a5298)';
  }
}

________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
