# Depto de Sistemas y Computación
# Ing. En Sistemas Computacionales
# SISTEMAS PROGRAMABLES 23a


# OBJETIVO:
Desplegar un hola mundo con el tiempo de un servidor NTP por medio de Wifi

# CÓDIGO
```python
## Depto de Sistemas y Computación
## Ing. En Sistemas Computacionales
## SISTEMAS PROGRAMABLES 23a
## Autor (es): Jesus Antonio Sanchez Santos
## Repositorio: https://github.com/antsz25
## Fecha de revisión:   17/10/2023
## Objetivo: Desplegar un hola mundo con el tiempo de un servidor NTP por medio de Wifi

# Imports
import time
import utime
import network  # for Wifi
from machine import Pin, I2C  # for LED
import ntptime
import ssd1306

# Global to check if time is set
global time_is_set
global wifi_is_connected

ssid = 'IZZI-FA06'
password = '9CC8FC60FA06'

def wifi_connect(ssid, password):
    global wifi_is_connected  # Declare wifi_is_connected as a global variable
    # Connect to WiFi
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(ssid, password)

    max_wait = 15
    while max_wait > 0:
        if wlan.status() < 0 or wlan.status() >= 3:
            break
        max_wait -= 1
        time.sleep(5)

    if wlan.status() != 3:
        raise RuntimeError('Network connection failed')
    else:
        wifi_is_connected = True
        status = wlan.ifconfig()
        led = Pin("LED", Pin.OUT)
        led.on()
        
def ReturnNTPTimeServer():
    # Sincronizar la hora con un servidor NTP
    ntptime.settime()
    # Get the current time as a tuple
    time_zone_offset = -7 * 3600  # 7 hours behind UTC in seconds
    # Synchronize the time with an NTP server
    current_time = utime.localtime(utime.mktime(utime.localtime()) + time_zone_offset)
    # Individual Components of the time
    year = current_time[0]
    month = current_time[1]
    day = current_time[2]
    hour = current_time[3]
    minute = current_time[4]
    second = current_time[5]
    day_of_week = current_time[6]
    day_of_year = current_time[7]
    Formated="{}:{}".format(hour,minute)
    i2c = I2C(0,scl=1,sda=0)
    display = ssd1306.SSD1306_I2C(128,64,i2c)
    if 0 <= hour <=12:
        display.fill(0)
        display.text("Hora:"+Formated,0,16)
        display.text("Hola Mundo",0,32)
        display.show()

    elif 12 < hour <=18:
        display.fill(0)
        display.text("Hora:"+Formated,0,16)
        display.text("Hola Mundo",0,32)
        display.show()
    else:
        display.fill(0)
        display.text("Hora:"+Formated,0,16)
        display.text("Hola Mundo",0,32)
        display.show()

        
while True:
    wifi_connect(ssid, password)
    ReturnNTPTimeServer()
    time.sleep(60)
    led.off()
    time.sleep(0.1)
```

# PRUEBAS

![Imagen_Prueba1](https://github.com/antsz25/2.1HolaMundoYNTP/blob/main/Prueba.jpg)

# CONCLUSIONES
Para ser una primera práctica es entretenido. La utilización del módulo de Wifi de la raspberry es de suma utilidad, ya que ahorra dinero y espacio en la utilización de otro módulo externo para poder cumplir con lo deseado. 
