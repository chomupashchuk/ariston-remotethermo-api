# Ariston NET remotethermo API
Thin integration is a side project which works only with 1 zone climate configured. It logs in to Ariston website (https://www.ariston-net.remotethermo.com) and fetches/sets data on that site.
You are free to modify and distribute it. It is distributed 'as is' with no liability for possible damage.

## API and Home Assistant
API was created in order to be used by Home Assistant officially. Example of API use can be found: 

## API slow nature
API connect to the website, which then connect via gateway to the boiler. The bus has problem handling high bandwidth and thus requests are sent after some specific periods of time. Periods were selected based on tests where not much of interfence was seen when using Ariston Net application or Google Home application or using https://www.ariston-net.remotethermo.com. Still interfences occaionally take place. It is normal to occasionally get connection errors due to devices chain involved.

## API was tested on and works with:
  - Ariston Clas Evo
  - Ariston Genus One with Ariston BCH cylinder
  - Ariston Nimbus Flex
You may check possible support of your boiler by logging into https://www.ariston-net.remotethermo.com and if climate and water heater parts (like temperatures) are available on the home page, then intergation should potentially work.

## API was tested and does not work with:
  - Ariston Velis Wifi
Website https://www.ariston-net.remotethermo.com does not provide required data for this kind of a boiler.

## API use
API was developed as `AristonHandler` class, which can be imported from the module. Use PIP to install the module:
```
pip install aristonremotethermo
```
Import api in python script:
```
from aristonremotethermo.ariston import AristonHandler
```

### API dependencies
  - `requests` - for HTTPS requests.
  
### API start communication
```
from aristonremotethermo.ariston import AristonHandler

ApiInstance = AristonHandler(
            username='username',
            password='password',
            sensors=list_of_sensors,
            units=units,
            store_file=False,
            store_folder="/config/custom_components/ariston"
        )

ApiInstance.start()
```
  - `username` - mandatory username
  - `password` - mandatory password
  - `sensors` - `list` of sensor names to be fetched by API (see `ApiInstance.supported_sensors_get` for supported list of sensors by API). Used to filter out requests and only use required onces thus reducing cycle to fetch all of the data.
  - `retries` - `int` number of retries to set the data until successful result. Default is `5`. Useful in case of unstable connections.
  - `polling` - `float` poling rate. Default is `1`. With increase of the value all timers to fetch or set the data are increased by corresponding value.
  - `store_file` - `bool` if to store files with reply data and some internal dictionaries for troubleshooting purposes. Default is `False`.
  - `store_folder` - `str` path to the folder that shall be used for storing of files. Default is location of API.
  - `units` - `str` value of `metric`, `imperial` or `auto`. Default is `metric`. Value `auto` insterts additional request to fetch the data thus reducing cycle to fetch all of the data.
  - `ch_and_dhw` - `bool` if CH and DHW can heat independently (often there is one heating element and valve controls which element is being heated). Default is `False`. 
  - `dhw_unknown_as_on` - `bool` of how to treat unknown value of `dhw_flame` sensor by the API. The value is actually never reported and is only approximated by the api and in one of the cases (CH is ON) the value shall be treated based on this parameter. Default is `True`


### API stop communication
```
ApiInstance.stop()
```

### API properties
  - `ApiInstance.available` - returns `bool` if API assumes connection to be working or not.
  - `ApiInstance.ch_available` - returns `bool` if API assumes connection to be working or not and if data is available to assume CH as working.
  - `ApiInstance.dhw_available` - returns `bool` if API assumes connection to be working or not and if data is available to assume DHW as working.
  - `ApiInstance.version` - returns `str` of API version.
  - `ApiInstance.supported_sensors_get` - returns `set` of supported sensor names by the API that can be potentially fetched.
  - `ApiInstance.supported_sensors_set` - returns `set` of supported sensor names by the API that can be potentially set.
  - `ApiInstance.sensor_values` - returns `dict` of sensors with sensor names as keys and value being dictionary with keys `value` (for the fetched value) and `units` (for used units).
  - `ApiInstance.supported_sensors_set_values` - returns `dict` of sensors that support change of data as keys (see `supported_sensors_set`). The value of the dictionary is either set with supported values or another dictionary with keys `min` (minimum value), `max` (maximum value) and `step` (value step). The provided data is available based on fetched data, meaning occasionally it might be empty.
  - `ApiInstance.setting_data` - returns `bool` if setting of data by the API is ongoing.

### API changing data on the server
```
ApiInstance.set_http_data(parameter1=value1,parameter2=value2,...)
```
Method accepts parameters to be changed (see `supported_sensors_set`) according to `supported_sensors_set_values`.

#### API changing data example
```
ApiInstance.set_http_data("mode"="winter","ch_mode"="scheduled")
```

### API supported sensors
**API cannot identify which senors shall be in fact supported by the heater model and thus invalid or empty values might be expected**
  - `account_ch_gas` - gas use summary for CH. 
  - `account_ch_electricity` - electricity use summary for CH. 
  - `account_dhw_gas` - gas use summary for DHW. 
  - `account_dhw_electricity` - electricity use summary for DHW. 
  - `ch_antifreeze_temperature` - CH antifreeze temperature.
  - `ch_detected_temperature` - temperature measured by thermostat.
  - `ch_mode` - mode of CH (`manual` or `scheduled` and others).
  - `ch_comfort_temperature` - CH comfort temperature.
  - `ch_economy_temperature` - CH economy temperature.
  - `ch_set_temperature` - CH temperature currently used as target.
  - `ch_program` - CH Time Program.
  - `dhw_program` - DHW Time Program.
  - `dhw_comfort_function` - DHW comfort function.
  - `dhw_mode` - mode of DHW. 
  - `dhw_comfort_temperature` - DHW storage comfort temperature. 
  - `dhw_economy_temperature` - DHW storage economy temperature. 
  - `dhw_set_temperature` - DHW temperature currently used as target.
  - `dhw_storage_temperature` - DHW storage temperature. 
  - `dhw_thermal_cleanse_cycle` - DHW thermal cleanse cycle.
  - `electricity_cost` - Electricity cost.
  - `errors` - `list` of active errors (no actual errors to test on).
  - `errors_count` - active errors (no actual errors to test on).
  - `gas_type` - Gas type.
  - `gas_cost` - Gas cost.
  - `heating_last_24h` - energy use for heating in last 24 hours as summary.
  - `heating_last_30d` - energy use for heating in last 7 days as summary. 
  - `heating_last_365d` - energy use for heating in last 30 days as summary. 
  - `heating_last_7d` - energy use for heating in last 365 days as summary. 
  - `heating_last_24h_list` - energy use for heating in last 24 hours as `dict` of different periods. 
  - `heating_last_30d_list` - energy use for heating in last 7 days as `dict` of different periods. 
  - `heating_last_365d_list` - energy use for heating in last 30 days as `dict` of different periods. 
  - `heating_last_7d_list` - energy use for heating in last 365 days as `dict` of different periods. 
  - `mode` - mode of boiler (`off` or `summer` or `winter` and others).
  - `outside_temperature` - outside temperature. 
  - `signal_strength` - Wifi signal strength.
  - `units` - Units of measurement.
  - `water_last_24h` - energy use for water in last 24 hours as summary. 
  - `water_last_30d` - energy use for water in last 7 days as summary. 
  - `water_last_365d` - energy use for water in last 30 days as summary. 
  - `water_last_7d` - energy use for water in last 365 days as summary. 
  - `water_last_24h_list` - energy use for water in last 24 hours as `dict` of different periods. 
  - `water_last_30d_list` - energy use for water in last 7 days as `dict` of different periods. 
  - `water_last_365d_list` - energy use for water in last 30 days as `dict` of different periods. 
  - `water_last_7d_list` - energy use for water in last 365 days as `dict` of different periods. 
  - `ch_auto_function` - CH AUTO function status.
  - `ch_flame` - CH heating ongoing.
  - `ch_pilot` - CH Pilot mode.
  - `dhw_flame` - DHW heating ongoing. **This parameter is not reported by boilers and is approximated based on multiple parameters**.
  - `dhw_thermal_cleanse_function` - DHW thermal cleanse function.
  - `flame` - CH and/or DHW heating ongoing.
  - `heat_pump` - Heating pump status.
  - `holiday_mode` - Holiday mode status.
  - `internet_time` - Internet time status.
  - `internet_weather` - Internet weather status.
  - `update` - API update status. **API specific sensor, which fetches latest version from github. Fetching from github is performed if sensor `update` or `online_version` in the list of sensors during class instance initiation**.
  - `online_version` - API online version. **API specific sensor, which fetches latest version from github. Fetching from github is performed if sensor `update` or `online_version` in the list of sensors during class instance initiation**.

### API supported parameters to be changed
**API cannot identify which parameters shall be in fact supported by the heater model and thus invalid or empty values might be expected in `supported_sensors_set_values`**
**All values expected to be a string** 
  - `mode` - mode of boiler (`off` or `summer` or `winter` and `heating_only`).
  - `internet_time` - turn off and on sync with internet time (`true` or `false`).
  - `internet_weather` - turn off and on fetching of weather from internet (`true` or `false`).
  - `ch_mode` - mode of CH (`manual` or `scheduled`).
  - `ch_auto_function` - turn off and on Auto function (`true` or `false`).
  - `ch_comfort_temperature` - CH comfort temperature.
  - `ch_economy_temperature` - CH economy temperature.
  - `ch_set_temperature` - CH temperature currently used as target.
  - `dhw_mode` - mode of DHW (`manual` or `scheduled`). 
  - `dhw_comfort_temperature` - DHW storage comfort temperature. 
  - `dhw_economy_temperature` - DHW storage economy temperature. 
  - `dhw_set_temperature` - DHW temperature currently used as target.
  - `dhw_thermal_cleanse_function` - DHW thermal cleanse function enabled (`true` or `false`).
  - `dhw_thermal_cleanse_cycle` - DHW thermal cleanse cycle.
  - `dhw_comfort_function` - DHW comfort function (`disabled` or `time_based` or `always_active`).
  - `units` - Units of measurement (`imperial` or `metric`).
