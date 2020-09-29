# Freepad

Freepad is an open source Zigbee remote intended to be used to have a customizable keypad to control your smart home devices.  
Read more here: https://modkam.ru/?p=1264

## Features list:
1. Single/double/tripple/quadriple/many_x/hold&release
2. Touchlink reset
3. ONOFF bind
4. Level control bind
5. Bindings configuration
6. Remote reset


## How to join:

1. Press and hold *any* button for 3-10 seconds(depends whter or not device is on a network), until device start flashing led
2. Wait, in case of successfull join, device will flash led 5 times
3. If join failed, device will flash led 3 times

## How to use touch link
 Deprecated due to memory issues, you can use `FREEPAD_ENABLE_TL` macros if you want to compile with TL functionality


## What's button mapping?
![Here](./images/zigbee_keypad22.png)

## How to add device into zigbe2mqtt
Should be already in dev branch (as of 19-05-2020)



## Work modes
By default remote works as custom swith, with multiple clicks, this behaiviout has own drawback.
In order to detect multiple clicks, remote sends commands with 300ms delay.
You can change this behaviour by cost of double/tripple/etc clicks. 
To do that you need to change

`ZCL_CLUSTER_ID_GEN_ON_OFF_SWITCH_CONFIG` cluster `ATTRID_ON_OFF_SWITCH_TYPE` attribute

Values are:

`ON_OFF_SWITCH_TYPE_MOMENTARY` (0) -> no delay, but no multiple clicks, only single

```bash
mosquitto_pub -t "zigbee2mqtt/FN/BUTTON_NUM/set/switch_type" -m '0'
```

`ON_OFF_SWITCH_TYPE_MULTIFUNCTION` (2) -> 300ms delay, full set of clicks

```bash
mosquitto_pub -t "zigbee2mqtt/FN/BUTTON_NUM/set/switch_type" -m '2'
```


## ONOFF cluster binding
By default command is TOGGLE, but you can change this behaviour

Change `ZCL_CLUSTER_ID_GEN_ON_OFF_SWITCH_CONFIG` clusters attribute `ATTRID_ON_OFF_SWITCH_ACTIONS`

`ON_OFF_SWITCH_ACTIONS_ON`

```bash
mosquitto_pub -t "zigbee2mqtt/FN/BUTTON_NUM/set/switch_actions" -m '0'
```

`ON_OFF_SWITCH_ACTIONS_OFF`

```bash
mosquitto_pub -t "zigbee2mqtt/FN/BUTTON_NUM/set/switch_actions" -m '1'
```

`ON_OFF_SWITCH_ACTIONS_TOGGLE` (default value)

```bash
mosquitto_pub -t "zigbee2mqtt/FN/BUTTON_NUM/set/switch_actions" -m '2'
```


## How to compile

Since the Project is based on the ZStack sample project, and this one comes with IAR project files,
you need IAR to compile the project.

1. Install ZStack (used 3.0.2)
2. Navigate to ```<ZSTACK_DIR>\Projects\zstack\HomeAutomation```
3. Checkout the project into that directory
4. Open ```<CHECKOUT_DIR>CC2530DB\GenericApp.eww``` with IAR (Project file)
5. Switch Poject Configuration via ```Project -> Edit Configurations...``` to ```DIYRuZ_FreePad_TL_PM3``` (with Touchlink and PowerManagement 3 (PM3???))
6. Execute ```Project -> Rebuild All```

Firmware file is located at ```<CHECKOUT_DIR>\firmwares```