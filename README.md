# Weather-App

1. Projekt
Aplikacja sluży do podawania aktualnej sytuacji pogodowej tj. temperaturę minimalną, maksymalną, prędkość wiatru, ciśnienie atmosferyczne, wilgotność itd.
Jest to bardzo łatwe i szybkie. Dodatkowo, przy niewielkiej zmianie kodu, można wybierać dane, które nas interesują.

2. Technologia

Aplikacja napisana jest w Pythonie 3.10.

3. Przed uruchomieniem.

pip install requests

4. Kod- objaśnienie

 a) ściąganie bibliotek i modułów potrzebnych do uruchomienia aplikacji

    import tkinter as tk        - biblioteka
    import requests     - biblioteka 
    import time     - moduł

 b) funkcja, która ściąga aktualną pogodę, ze strony internetowej: "https://openweathermap.org/" 
 
    def getWeather(canvas):
    city = textfield.get()
    api = "https://api.openweathermap.org/data/2.5/weather?q=" + city +"&appid=e8c3934dc403a7ddadca68032f5fbb33"        - podanie ścieżki pozyskania danych pogodowych
    json_data = requests.get(api).json()
    condition = json_data['weather'][0]['main']
    temp = int(json_data['main']['temp'] - 273.15)
    min_temp = int(json_data['main']['temp_min'] - 273.15)      - zmiana temperatury na stopnie Celcjusza
    max_temp = int(json_data['main']['temp_max'] - 273.15)      - zmiana temperatury na stopnie Celcjusza
    pressure = json_data['main']['pressure']
    humidity = json_data['main']['humidity']
    wind = int(json_data['wind']['speed'] * 3.6)        - zmiana prędkości wiatru z m/s na km/h
    sunrise = time.strftime("%D %H:%M:%S", time.gmtime(json_data['sys']['sunrise'] +7200 ))
    sunset = time.strftime("%D %H:%M:%S", time.gmtime(json_data['sys']['sunset'] +7200 ))

    - Dalsza część funkcji, która odpowiada za ostateczny wygląd przedstawionych danych

    final_info = condition + "\n" + str(temp) + "°C"
    final_data = "\n" + "Max Temp: " + str(max_temp) + "\n" + "Min Temp: " + str(min_temp) + "\n" + "Pressure: " + str(pressure) + "\n" "Humidity :" + str(humidity) + "\n" + "Wind Speed: " + str(wind) + "\n" + "Sunrise" + sunrise + "\n" + "Sunset: " + sunset
    label1.config(text = final_info)
    label2.config(text = final_data)

 c) kod odpowiadający za rysowanie okna, w którym znajdują sie dane pogodowe: nazwa aplikacji

    canvas = tk.Tk()
    canvas.geometry("600x500")      - rozmiar okna
    canvas.title("Weather App")     - tytuł

    f = ("poppins", 15, "bold")     - rodzaj i wielkość czcionki
    t = ("poppins", 35, "bold")     - rodzaj i wielkość czcionki

    textfield = tk.Entry(canvas, font = t)      - wykorzystanie wcześniej określonej czcionki "t"
    textfield.pack(pady = 20)       - odległość paska, do którego wpisujemy nazwę miasta
    textfield.focus()
    textfield.bind('<Return>', getWeather)

    label1 = tk.Label(canvas, font = t)      - wykorzystanie wcześniej określonej czcionki "t"
    label1.pack()
    label2 = tk.Label(canvas, font = f)      - wykorzystanie wcześniej określonej czcionki "f"
    label2.pack()

    canvas.mainloop()
