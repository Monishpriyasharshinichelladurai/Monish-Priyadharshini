from flask import Flask, render_template, request
import requests

app = Flask(__name__)

# OpenWeather API Key (replace with your key)
API_KEY = "d9577b52637844c8162a8fcb5fc42584"

@app.route("/", methods=["GET", "POST"])
def index():
    weather_data = None
    error_message = None

    if request.method == "POST":
        city = request.form["city"]
        url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}&units=metric"
        
        response = requests.get(url)
        if response.status_code == 200:
            weather_data = response.json()
        else:
            error_message = "City not found. Please try again."

    return render_template("index.html", weather=weather_data, error=error_message)

if __name__ == "__main__":
    app.run(debug=True)