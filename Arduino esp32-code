from umqtt.simple import MQTTClient
import machine

# Configuration
MQTT_SERVER = 'broker.hivemq.com'
MQTT_TOPIC = 'lumiflow/control'

# Motor and Valve Control Pins
valve_pin = machine.Pin(14, machine.Pin.OUT)
light_pin = machine.PWM(machine.Pin(12))

def on_message(topic, msg):
    message = msg.decode('utf-8')
    print(f"Message received: {message}")
    
    if "valve:open" in message:
        valve_pin.value(1)
    elif "valve:close" in message:
        valve_pin.value(0)
    elif "light_intensity" in message:
        intensity = int(message.split(":")[1])
        light_pin.duty(intensity * 10)  # PWM from 0 to 1023
    elif "lift_plate:yes" in message:
        # Control lifting mechanism here
        pass

client = MQTTClient("ESP32_LumiFlow", MQTT_SERVER)
client.set_callback(on_message)
client.connect()
client.subscribe(MQTT_TOPIC)

while True:
    client.wait_msg()
