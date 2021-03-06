# boiler_control
This is about Buderos Logamax U072 boiler automation using Arduino-based OpenTherm gateway (made out of dirt, tears and blue insulating tape) and gorgeous Home Assistant.

## Modus operandi 
There are several input parameters: inside air temperature in different rooms of the house, air temperature ourside, coolant (heatant?) temperature, several configuration parameters.
All this goes into a regulator, with the only purpose: to keep inside air temperature as close to the set point as possible.
There are several limitations caused by the imperfection of the real heating system. For example, in my house there is a coolant splitter burried deep inside the wall. If the coolant temperature drops below, say, 17C, it starts to leak, so I the regulator must keep the coolant (c'mon, I will use word "heatant") at at least of that temperature.

So the regulator gets all the input data (inside temperature, outside temperature, heatant temperature, configuration parameters), does some magic math and generates required heatant setpoint.
This setpoint is sent to the boiler hardware using Arduino-based (include face-palm picture) OpenTherm gateway.
At the same time, the OTGW (OpenTherm gateway) reads current status from the boiler.

## Regulator settings
min_boiler_setpoint

max_boiler_setpoint

off_boiler_setpoint

min_outside_temp

max_outside_temp

min_boiler_temp

inside_temp_setpoint

inside_temp_hyst

off_boiler_setpoint


## Regulator input values
Array of room1_temp, room2_temp, ...

outside_temp

## Boiler monitoring values
boiler_heatant_temp (int)

boiler_flame_on (boolean)

boiler_modulation (0-100%)

boiler_connected (boolean)


## Regulator algorythim
// Required heatant temp calculation. Output value: boiler_setpoint

if outside_temp < min_outside_temp { return max_boiler_setpoint }

if outside_temp > max_outside_temp { return min_boiler_setpoint }

return (max_boiler_temp - min_boiler_temp) / (max_outside_temp - min_outside_temp) * (max_outside_temp - outside_temp) + min_boiler_temp

// To heat or not to heat decision

if boiler_heatant_temp < min_boiler_temp { return True}

if min_room_temp > (inside_temp_setpoint + inside_temp_hyst) { return False }

return True

// Main cycle

mqtt.get_all_data()

ot.get_all_data()

rht = calculate_heatant_temp() // Required heatant temperature

if not heat_decision() { rht = off_boiler_setpoint }

ot.post_rht()

mqtt.post_all_data()

