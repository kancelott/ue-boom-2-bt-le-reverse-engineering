# UE BOOM 2 BT LE Reverse Engineering

Bluetooth LE GATT details for the UE BOOM 2, in an attempt to remotely power it on/off and also get the battery level.

## Credits

Thanks to:
[https://www.reddit.com/r/bluetooth/comments/ap6npx/bluetooth_protocol_for_ue_boom_2/](https://www.reddit.com/r/bluetooth/comments/ap6npx/bluetooth_protocol_for_ue_boom_2/)

and: [https://www.reddit.com/r/shortcuts/comments/dz9zun/finally_turn_on_ue_boom_bluetooth_speaker/](https://www.reddit.com/r/shortcuts/comments/dz9zun/finally_turn_on_ue_boom_bluetooth_speaker/)


## Interesting GATT UUIDs from UE BOOM apk (from Reddit)
```
/* compiled from: BLECommand.kt */

public enum BLECommand {

POWER_ON("c6d6dc0d-07f5-47ef-9b59-630622b01fd3", (int) true),
BATTERY_LEVEL("00002a19-0000-1000-8000-00805f9b34fb", (int) false),
NAME("00002a00-0000-1000-8000-00805f9b34fb", (int) false),
FW_VERSION("00002a28-0000-1000-8000-00805f9b34fb", (int) false),
HW_VERSION((String) (short) 10791, (int) false),
SERIAL("00002a25-0000-1000-8000-00805f9b34fb", (int) false),
COLOR("54F7F292-7EBB-4267-83C2-8E6EE7E881FF", (int) false),
ALARM("16E005BB-3862-43C7-8F5C-6F654A4FFDD2", (int) true),
COLOR_ADK3((String) (short) 10788, (int) false);

```


## Power on/off
```
# substitute an authorised BT MAC (YYYYYYYYYYYY)

# turn on
sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-write-req -a 0x0003 -n YYYYYYYYYYYY01

# turn off
sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-write-req -a 0x0003 -n YYYYYYYYYYYY02
```

handle: 0x0002, char properties: 0x08, char value handle: 0x0003, uuid: c6d6dc0d-07f5-47ef-9b59-630622b01fd3


## Getting battery level
```
# substitute an authorised BT MAC (YYYYYYYYYYYY)

# get battery
sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0014
Characteristic value/descriptor: 5b
```

handle: 0x0013, char properties: 0x02, char value handle: 0x0014, uuid: 00002a19-0000-1000-8000-00805f9b34fb


## Others
```
# ON / OFF
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0003
Characteristic value/descriptor read failed: Attribute can't be read

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0006
Characteristic value/descriptor: 44 57

# ALARM
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0009
Characteristic value/descriptor: 00 00 00 00 7c 13 09 0f 00 00 00 00 1e 0a

# COLOUR
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x000c
Characteristic value/descriptor: a5 00

# FW_VERSION
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x000e
Characteristic value/descriptor: 02 04 07 00

# SERIAL
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0010
Characteristic value/descriptor: 31 37 33 35 4c 5a 30 37 4c 4d 4d 38 00

# NAME
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0012
Characteristic value/descriptor: 55 45 20 42 4f 4f 4d 20 32 00

# BATTERY
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0014
Characteristic value/descriptor: 5d

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0016
Characteristic value/descriptor: 40

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0018
Characteristic value/descriptor: 42

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x001a
Characteristic value/descriptor: 04

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x001c
Characteristic value/descriptor: 02

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x001e
Characteristic value/descriptor: 03

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0020
^C

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0022
Characteristic value/descriptor read failed: Attribute can't be read

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0024
Characteristic value/descriptor: 00

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0026
connect error: Function not implemented (38)

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x0028
Characteristic value/descriptor: 00 00 00 00 00 00 00 00 00 00

# ???
pi@peach-pi:~ $ sudo gatttool -i hci0 -b C0:28:8D:XX:XX:XX --char-read -a 0x002a
Characteristic value/descriptor: 20

```

## bluez commands
```
# scan for BTLE devices
$ sudo hcitool lescan

# reset
$ sudo hciconfig hci0 reset

# UE BOOM 2
$ sudo hcitool leinfo C0:28:8D:XX:XX:XX
Requesting information ...
    Handle: 64 (0x0040)
    Features: 0x00 0x00 0x00 0x00 0x00 0x00 0x00 0x00

# GATT characteristics
$ sudo gatttool -I
[                 ][LE]> connect  C0:28:8D:XX:XX:XX
Attempting to connect to C0:28:8D:XX:XX:XX
Connection successful
[C0:28:8D:XX:XX:XX][LE]> characteristics
handle: 0x0002, char properties: 0x08, char value handle: 0x0003, uuid: c6d6dc0d-07f5-47ef-9b59-630622b01fd3 # ON/OFF
handle: 0x0005, char properties: 0x0a, char value handle: 0x0006, uuid: 16e009bb-3862-43c7-8f5c-6f654a4ffdd2
handle: 0x0008, char properties: 0x0a, char value handle: 0x0009, uuid: 16e005bb-3862-43c7-8f5c-6f654a4ffdd2 # ALARM
handle: 0x000b, char properties: 0x02, char value handle: 0x000c, uuid: 54f7f292-7ebb-4267-83c2-8e6ee7e881ff # COLOUR
handle: 0x000d, char properties: 0x02, char value handle: 0x000e, uuid: 00002a28-0000-1000-8000-00805f9b34fb # FW_VERSION
handle: 0x000f, char properties: 0x02, char value handle: 0x0010, uuid: 00002a25-0000-1000-8000-00805f9b34fb # SERIAL
handle: 0x0011, char properties: 0x02, char value handle: 0x0012, uuid: 00002a00-0000-1000-8000-00805f9b34fb
handle: 0x0013, char properties: 0x02, char value handle: 0x0014, uuid: 00002a19-0000-1000-8000-00805f9b34fb # BATTERY
handle: 0x0015, char properties: 0x02, char value handle: 0x0016, uuid: 4356a21c-a599-4b94-a1c8-4b91fca02a9a
handle: 0x0017, char properties: 0x0a, char value handle: 0x0018, uuid: 03ed06ef-6f39-4b62-8143-e2cc6d8a3cd9
handle: 0x0019, char properties: 0x02, char value handle: 0x001a, uuid: 765c2f02-4292-45c6-b69c-725e131f97a2
handle: 0x001b, char properties: 0x02, char value handle: 0x001c, uuid: 00002a27-0000-1000-8000-00805f9b34fb
handle: 0x001d, char properties: 0x02, char value handle: 0x001e, uuid: 015c9ee9-f3bf-4f2c-a35b-58267432c9ca
handle: 0x001f, char properties: 0x0a, char value handle: 0x0020, uuid: 69c0f621-1354-4cf8-98a6-328b8faa1897
handle: 0x0021, char properties: 0x08, char value handle: 0x0022, uuid: 8da149b6-5d82-11e5-885d-feff819cdc9f
handle: 0x0023, char properties: 0x0a, char value handle: 0x0024, uuid: fd6c01c5-25d5-4cd0-8806-52486311bd32
handle: 0x0025, char properties: 0x08, char value handle: 0x0026, uuid: b8387326-c355-4efc-b258-a67a38c00efd
handle: 0x0027, char properties: 0x02, char value handle: 0x0028, uuid: 5e72ccf6-f526-4bfb-b334-0204a75c84ff
handle: 0x0029, char properties: 0x02, char value handle: 0x002a, uuid: 00002a1a-0000-1000-8000-00805f9b34fb

[C0:28:8D:XX:XX:XX][LE]> char-read-hnd 14
Characteristic value/descriptor: 5b
```
