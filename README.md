# Ariston NET remotethermo API
Thin integration is a side project which works only with 1 zone climate configured. It logs in to Ariston website (https://www.ariston-net.remotethermo.com) and fetches/sets data on that site.
You are free to modify and distribute it. It is distributed 'as is' with no liability for possible damage.

## API and Home Assistant
API was created in order to be used by Home Assistant. Example of API use for Home Assistant can be found: https://github.com/chomupashchuk/ariston-remotethermo-home-assistant-v2

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
### API import
Install package:
```
pip install aristonremotethermo
```
Import class `AristonHandler`:
```
from aristonremotethermo.ariston import AristonHandler
```

### API dependencies
  - `requests` - used for HTTPS requests towards https://www.ariston-net.remotethermo.com.
  
### API start communication
```
from aristonremotethermo.ariston import AristonHandler

ApiInstance = AristonHandler(
            username='username',
            password='password'
        )

ApiInstance.start()
```
See `help(AristonHandler)` on how to properly initiate API.


### API stop communication
```
ApiInstance.stop()
```

### API properties
See `help(AristonHandler)`.

### API change of data on remote server
```
ApiInstance.set_http_data(parameter1=value1,parameter2=value2,...)
```
Method sets values for spefific parameter names (see proprerty `supported_sensors_set_values` from `help(AristonHandler)`) on the remote server.

#### API change of data example
```
ApiInstance.set_http_data("mode"="winter","ch_mode"="scheduled")
```