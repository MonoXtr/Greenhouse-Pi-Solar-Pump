# Greenhouse-Pi-Solar-Pump
Instructions for building and coding a greenhouse solar pump controlled by a Pi.

I have built and written the code for a solar heating pump for a greenhouse and I will share the important steps here. The project was originally created for an Internet of Things class but it has been extremely helpful so far and so I figured I would share it with anyone that's interested. 

Greenhouse Description: This pump was designed for use in heating water barrels in a custom built greenhouse. 
In this section I will demonstrate the layout of the greenhouse and provide some pictures for reference. This section can be skipped if you are not interested in seeing my personal setup.

Here is a picture of the outside of the greenhouse along with the attic where the sensors were placed on a coil of hose:
![image](https://user-images.githubusercontent.com/71414550/120868288-f5fd3600-c550-11eb-829d-2dd2470e4c06.png)
![image](https://user-images.githubusercontent.com/71414550/120869189-1cbc6c00-c553-11eb-9c67-4ad6177facb0.png)

Here is a picture of the interior of the greenhouse with the water barrels:

![image](https://user-images.githubusercontent.com/71414550/120869210-29d95b00-c553-11eb-8769-26ed75cabd42.png)

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

Once the initial setup has been completed you will need to setup the sensors. 
To do this follow the instructions provided in this link: https://www.raspberrypi-spy.co.uk/2013/03/raspberry-pi-1-wire-digital-thermometer-sensor/ You will need to use the setup provided and appropriate wires to recreate the picture of the breadboard provided. Here are a few reference images of my personal setup.
![image](https://user-images.githubusercontent.com/71414550/120537424-86494880-c3a2-11eb-8b8b-1233555fa8c4.png) 
![image](https://user-images.githubusercontent.com/71414550/120537477-96f9be80-c3a2-11eb-8a04-6573b2cc022e.png)

This link can also be used if the previous link is unclear: https://www.cl.cam.ac.uk/projects/raspberrypi/tutorials/temperature/

Once the sensor has been successfully setup for the Pi the Pi will need to be connected to the relay. This is a simple process which requires two wires connected from the Pi to the green section of the pump. You can see the connections in the previous reference images. 
Once the Pi is connected to the relay I would highly suggest that you test the code before going through the work of setting up your system. When I tested the project to make it work I used a string of Christmas lights to see if the code was running properly. Lights are a great way to test as you can easily see them turning off or on. The below code can be used for 1 or more sensors and the individual values can easily be changed by simply inserting your own numbers for your project. 
You can run this code easily in the default python program provided on the Pi once it is setup. This code also includes a section for using IFTTT to send your pump information to you. Instructions for using IFTTT can be found here: https://anthscomputercave.com/tutorials/ifttt/using_ifttt_web_request_email.html#:~:text=Log%20into%20IFTTT%20then%20go,the%20Subject%20and%20body%20fields If you do not need this feature you should be able to remove that part of the code. 
If this section is used it will require personal customization as my information will not be the same as yours.
