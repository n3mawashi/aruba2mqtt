# aruba2mqtt

This works for passive ble devices like Xiaomi thermometers but not for Flora/Flower Care

supported:
LYWSD03MMC ( temp/humidity sensor )


## Installation

Create protbuf files
- Download [protoc](https://github.com/protocolbuffers/protobuf/releases)

``` text
git submodule update --init --recursive

./protoc -I=./aos8-iot-server-example-websocket/proto_files/source/ --python_out=. --pyi_out=./ ./aos8-iot-server-example-websocket/proto_files/source/aruba-iot-* 

pip3 install -r requirements.txt
```

## Usage
```
python3 main.py
```

### Demo
![img.png](img.png)

## Aruba 8.12+ AoS configuration

replace <ip_address> with ip address of where you're running aruba2mqtt. The websocket 
URL appears to require something after ":7443/" before the AP will connect

```
configure terminal

iot radio-profile ble-both
  radio-mode ble
  ble-console dynamic
  ble-txpower -40

iot transportProfile aruba2mqtt
  endpointURL ws://<ipaddress>:7443/aruba
  endpointType telemetry-websocket
  payloadContent all
  payloadContent unclassified
  endpointToken 1234
  endpointID arubaiap
  transportInterval 30
  bleDataForwarding

iot use-radio-profile ble-both

iot useTransportProfile aruba2mqtt
```

## Aruba AoS 8 debug commands
```
show iot transportProfile
show ap debug ble-table all 
show ap debug ble-relay report
```
### Home Assistant MQTT Configuration
`configuration.yaml`
```
mqtt:
  - sensor:
      name: "ATC_XXXXXX Temperature"
      state_topic: "aruba2mqtt/ATC_XXXXX/state"
      unit_of_measurement: "°C"
      icon: "mdi:thermometer"
      value_template: "{{ value_json.temperature | round(1) }}"

  - sensor:
      name: "ATC_XXXXXX Humidity"
      state_topic: "aruba2mqtt/ATC_XXXXXX/state"
      unit_of_measurement: "%"
      icon: "mdi:water-percent"
      value_template: "{{ value_json.humidity | round(1) }}"
```

    {
[aruba2mqtt] |       "mac": "xHyNbI0J",
[aruba2mqtt] |       "deviceClass": [
[aruba2mqtt] |         "unclassified"
[aruba2mqtt] |       ],
[aruba2mqtt] |       "lastSeen": "1755334834",
[aruba2mqtt] |       "bevent": {
[aruba2mqtt] |         "event": "update"
[aruba2mqtt] |       },
[aruba2mqtt] |       "rssi": {
[aruba2mqtt] |         "avg": -72
[aruba2mqtt] |       },
[aruba2mqtt] |       "stats": {
[aruba2mqtt] |         "frameCnt": 14
[aruba2mqtt] |       },
[aruba2mqtt] |       "localName": "Flower care"
[aruba2mqtt] |     }, 