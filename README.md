# boiler_control
This is about Buderos Logamax U072 boiler automation using Arduino-based OpenTherm gateway (made out of dirt, tears and blue insulating tape) and gorgeous Home Assistant.

## Modus operandi 
There are several input parameters: inside air temperature in different rooms of the house, air temperature ourside, coolant (heatant?) temperature, several configuration parameters.
All this goes into a regulator, with the only purpose: to keep inside air temperature as close to the set point as possible.
There are several limitations caused by the imperfection of the real heating system. For example, in my house there is a coolant splitter burried deep inside the wall. If the coolant temperature drops below, say, 17C, it starts to leak, so I the regulator must keep the coolant (c'mon, I will use word "heatant") at at least of that temperature.

So the regulator gets all the input data (inside temperature, outside temperature, heatant temperature, configuration parameters), does some magic math and generates required heatant setpoint.
This setpoint is sent to the boiler hardware using Arduino-based (include face-palm picture) OpenTherm gateway.
At the same time, the OTGW (OpenTherm gateway) reads current status from the boiler.



