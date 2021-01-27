# LoRaWAN-thesis-work
import time
import ttn

app_id = "my-first-lorawan-app"
access_key = "ttn-account-v2.xiOtpU2OuGsjfVhxFe_1PLhjz3SUUDiySXamUikgJbA"

dev_id = "my-first-lorawan-device"
payload = "HELLO WORLD".encode('hex')

handler = ttn.HandlerClient(app_id, access_key)


# callbacks
def connect_callback(res, client):
  if res:
    print("Successfully connected")
  else:
    print("Unable to connect")	
    
def uplink_callback(msg, client):
  print("Received uplink from ", msg.dev_id)
  print(msg)
    
def downlink_callback(mid, client):
  print("Received downlink from ", mid)

def close_callback(res, client):
  if res:
    print("Successfully disconnected")
  else:
    print("Unable to disconnect")	

	
# using mqtt client
mqtt_client = handler.data()

# set callbacks
mqtt_client.set_connect_callback(connect_callback)
mqtt_client.set_uplink_callback(uplink_callback)
mqtt_client.set_downlink_callback(downlink_callback)
mqtt_client.set_close_callback(close_callback)

# connection request
mqtt_client.connect()
time.sleep(2)

# send data
mqtt_client.send(dev_id, payload, port=1, conf=False, sched="replace")
time.sleep(2)

# close connection
mqtt_client.close()
time.sleep(2)


# using application manager client
app_client =  handler.application()
my_app = app_client.get()
print(my_app)
my_devices = app_client.devices()
print(my_devices)
