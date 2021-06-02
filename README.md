# Greenhouse-Pi-Solar-Pump
Instructions for building and coding a greenhouse solar pump controlled by a Pi.

I have built and written the code for a solar heating pump for a greenhouse and I will share the important steps here. The project was originally created for an Internet of Things class but it has been extremely helpful so far and so I figured I would share it with anyone that's interested. 

Project Physical Requirements:

DS18B20 Temperature Sensors https://www.amazon.com/gp/product/B07782SXCZ/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1 
A basic set of male to female and male to male wires
Basic breadboard
4.7k Ohm Resistor 
Raspberry Pi (I used a model 4 B) https://www.amazon.com/gp/product/B07TD42S27/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
Raspberry Pi Power Supply https://www.amazon.com/gp/product/B07W93C4Z9/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
A Computer Monitor to connect to the Pi to see what you are doing (you can use a monitor you already own)
A keyboard and mouse to use the Pi (you can use a keyboard or mouse you already own)
Micro SD Card https://www.amazon.com/gp/product/B07Z7V34RG/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
IoT Relay https://www.amazon.com/Iot-Relay-Enclosed-High-Power-Raspberry/dp/B00WV7GMA2/ref=sr_1_2?dchild=1&keywords=iot+pump+relay&qid=1622658822&sr=8-2

Pi Starter Kit (Includes resistor, breadboard, and wires as well as many other extra components, these pieces can also be purchased seperately): https://www.amazon.com/gp/product/B088QZSGD1/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1

Before you begin the project you will need to make sure that your Pi is properly configured. To properly set up your Pi for use follow the following instructions: https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up

Once the initial setup has been completed you will need to setup the sensors. To do this follow the instructions provided in this link: https://www.raspberrypi-spy.co.uk/2013/03/raspberry-pi-1-wire-digital-thermometer-sensor/ You will need to use the setup provided and appropriate wires to recreate the picture of the breadboard provided. Here are a few reference images of my personal setup.![image](https://user-images.githubusercontent.com/71414550/120537424-86494880-c3a2-11eb-8b8b-1233555fa8c4.png) ![image](https://user-images.githubusercontent.com/71414550/120537477-96f9be80-c3a2-11eb-8a04-6573b2cc022e.png)
This link can also be used if the previous link is unclear: https://www.cl.cam.ac.uk/projects/raspberrypi/tutorials/temperature/

Once the sensor has been successfully setup for the Pi the Pi will need to be connected to the pump. This is simple process which requires two wires connected from the Pi to the green section of the pump. You can see the connections in the previous reference images. 
 
 
 
 
 
 
 
 
 
 
 
 
 
My Pi Pump Code:
 
import time
import glob
import requests
from datetime import datetime
import os.path
from os import path
import array as arr
from gpiozero import LED
from time import sleep

pump=LED(21)
tarray=arr.array('f', [])
t2array=arr.array('f', [])
print(tarray)
##Control Parameters
ct=10 ##check time(sleep)
dt=4 ##delta temp
dt_off=0.5 ##delta temp for pump off
dti=240 ##delay time for starting
dti_off=600 ##delay time to turn off
wi=5 ##write increment, # of check times between writes

oc=-ct ##on counter(time delay starting
offc=-ct
po = bool(False) ##pump on(status)
index=0
pc=0 ##print counter for tracking loops and writing
current_date = datetime.now() ##Declaring date and time
current_date_string = str(current_date) ##Turning to string
extension = ".txt" ##Extension

filename=" "

def email_alert(first, second, third):
    print("In Email alert")
    report = {}
    print("declared report")
    report["value1"] = first
    report["value2"] = second
    report["value3"] = third
    print("Entering try")
    try:
        requests.post("https://maker.ifttt.com/trigger/Greenhouse_Pump/with/key/d5_b4h9ANC-pPQK-00QrTX", data=report)
        print("Email sent!")
    except:
        print("Email sending error")

##a = "PUMP ON"
##print("Choose your second string.")
##b =  ##tarray[0]
##print("Choose your third string.")
##c = print("Temp 2 Here") ##tarray[1]
   

while True:
   for sensor in glob.glob("/sys/bus/w1/devices/28-01*/w1_slave"):
      id = sensor.split("/")[5]
      try:
         f = open(sensor, "r")
         data = f.read()
         f.close()
         if "YES" in data:
            (discard, sep, reading) = data.partition(' t=')
            t = float(reading) / 1000.0
            print("{} {:.1f}".format(id, t))
            tarray.append(t)
           
            ++index
         else:
            print("Error")
            t = -100.0
            tarray.append(t)

      except:
         pass
   
   print(tarray)
   ##print(t2array)
   time.sleep(ct)
 
   if((tarray[2]-tarray[1])>dt):
        oc+=ct
        offc=-ct
   elif((tarray[2]-tarray[1])<dt_off):
        offc+=ct
        oc=-ct
   else:
        oc=-ct
        offc=-ct
       
   if(oc>=dti and po==False):
        print("Pump on!")
        po = bool(True)
        pump.on()
        print("Calling Email alert")
        email_alert("PUMP ON", str(tarray[2]), str(tarray[1]))
        print("Called Email alert")
       
        offc=-ct
        ##a = "PUMP ON"
        ##b =  ##tarray[0]
        ##c = print("Temp 2 Here") ##tarray[1]
   if(offc>=dti_off and po):
        print("Pump off!")
        po = bool(False)
        pump.off()
        email_alert("PUMP OFF", str(tarray[2]), str(tarray[1]))
       
        oc=-ct
   
   pc+=1
   if(pc>=wi):
       datetime_temp=datetime.now()
       date=datetime_temp.strftime("%y"+"-"+"%m"+"-"+"%d")
       current_time=datetime_temp.strftime("%X")
       #date=current_date.strftime("%y"+"-"+"%m"+"-"+"%d")
       #current_time=current_date.strftime("%X")
       previous_filename=filename
       filename = "Pump Info "+date+ extension
       if os.path.isfile(filename):  
           header=""
   
       else:
           header="Water Temp (C), Coil Temp (C), Pump On, Time, Air Temp (C)\n"
   
       f = open(filename, "a+")
       f.write(header)
       f.write(str(tarray[1]))
       f.write(', ')
       f.write(str(tarray[2]))
       f.write(str(', '))
       f.write(str(po))
       f.write(str(', '))
       f.write(current_time)
       f.write(str(', '))
       f.write(str(tarray[0])+"\n")
       f.close()


       pc=0
   tarray=arr.array('f', [])
   ##print(t2array)
