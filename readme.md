A fully off-grid Tesla Charging Automation designed to maximize charging with excess solar power.

Requirements: Tessie or another 3rd party plugin giving access to your tesla.

This Home Assistant blueprint allows you to adjust the charging current for your Tesla based on battery state of charge (SOC), comparison of PV and AC loads, and predefined maximum current limits based on SOC. 
The blueprint is designed to be highly configurable, allowing users to select their entities, thresholds, and battery levels.

Since HA won't toggle an entity that is already in that state, this relies on that to minimize API calls to your Tesla if the charger is ALREADY in the desired position (either on or off).

## Features

- Adjusts charging current based on battery SOC.
- Considers PV and AC loads to dynamically change charging rates.
- Sets maximum current limits based on SOC caps.
- Turns off charging when the charge cable is not connected.

## Inputs

- **Charge Cable Entity**: The entity representing the charge cable status.
- **Battery SOC Sensor**: The entity representing the battery state of charge.
- **VE.Bus Alarm**: The entity representing the VE.Bus alarm status.
- **Charge Current Number**: The entity representing the charge current.
- **Charge Switch**: The switch to control charging.
- **PV Watts Sensor**: The entity representing the PV watts.
- **AC Loads Sensor**: The entity representing the AC loads.
- **Increase Threshold**: The threshold for increasing the charging current (default: 0 W).
- **Maintain Threshold**: The threshold for maintaining the charging current (default: 100 W).
- **Decrease Threshold**: The threshold for decreasing the charging current (default: 200 W).
- **SOC Cap 95%**: Maximum charging current for SOC below 95% (default: 1 A).
- **SOC Cap 97%**: Maximum charging current for SOC between 95% and 97% (default: 2 A).
- **SOC Cap 99%**: Maximum charging current for SOC between 97% and 99% (default: 5 A).
- **SOC Cap 100%**: Maximum charging current for SOC above 99% (default: 10 A).

## How It Works

1. The blueprint triggers every minute to check the conditions.
2. If the charge cable is not connected, the sequence stops.
3. If the battery SOC is below 93%, the charging is turned off.
4. If the VE.Bus alarm is in a WARNING state, the charging current is reduced by 2 A.
5. If the charge current is unknown, it is set to 1 A, and charging is turned off.
6. Based on the battery SOC, different charging current limits are set:
   - Below 95% SOC: Charging current is set to the SOC Cap 95% value.
   - Between 95% and 97% SOC: Charging current is dynamically adjusted within the SOC Cap 97% value.
   - Between 97% and 99% SOC: Charging current is dynamically adjusted within the SOC Cap 99% value.
   - Above 99% SOC: Charging current is dynamically adjusted within the SOC Cap 100% value.
