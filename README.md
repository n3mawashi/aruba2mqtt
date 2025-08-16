# aruba2mqtt

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

## Aruba 8 AoS configuration

reaplace <ip_address> with ip address of where you're running aruba2mqtt
```
configure terminal

iot radio-profile "ble-scan"
    radio-mode ble
    ble-opmode scanning
    exit

  iot use-radio-profile "ble-scan"

  iot transportProfile "ble-ws"
    endpointType telemetry-websocket
    endpointURL "ws://<ip_address>:7443/"
    endpointToken "12345"
    payloadContent all
    bleDataForwarding
    transportInterval 30
    exit
  iot useTransportProfile "ble-ws"
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