# Ariston NET remotethermo API
Thin integration is a side project which works only with 1 zone climate configured. It logs in to Ariston website (https://www.ariston-net.remotethermo.com) and fetches/sets data on that site.
You are free to modify and distribute it. It is distributed 'as is' with no liability for possible damage.
See also https://pypi.org/project/aristonremotethermo/

## Donations
If you like this app, please consider donating some sum to your local charity organizations or global organization like Red Cross. I don't mind receiving donations myself (you may conact me for more details if you want to), but please consider charity at first.

## API and Home Assistant
API was created in order to be used by Home Assistant. Example of API use for Home Assistant can be found: https://github.com/chomupashchuk/ariston-remotethermo-home-assistant-v2 and https://github.com/chomupashchuk/ariston-aqua-remotethermo-home-assistant

## API slow nature
API connect to the website, which then connect via gateway to the boiler. The bus has problem handling high bandwidth and thus requests are sent after some specific periods of time. Periods were selected based on tests where not much of interfence was seen when using Ariston Net application or Google Home application or using https://www.ariston-net.remotethermo.com. Still interfences occaionally take place. It is normal to occasionally get connection errors due to devices chain involved.

## AristonHandler was tested on and works with:
  - Ariston Clas Evo
  - Ariston Genus One with Ariston BCH cylinder
  - Ariston Nimbus Flex
  - Ariston Alteas One

## AquaAristonHandler was tested works with:
  - Ariston Velis
  - Ariston Lydos
  - Ariston Lydos Hybrid

## Check which version to use
You may check possible support of your boiler by logging into https://www.ariston-net.remotethermo.com and if climate and water heater parts (like temperatures) are available on the home page, then the integration should potentially work with AristonHandler.

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
Import class `AquaAristonHandler`:
```
from aristonremotethermo.aristonaqua import AquaAristonHandler
```

### API dependencies
  - `requests` - used for HTTPS requests towards https://www.ariston-net.remotethermo.com.
  
### AristonHandler start communication
```
from aristonremotethermo.ariston import AristonHandler

ApiInstance = AristonHandler(
            username='username',
            password='password'
        )

ApiInstance.start()
```
See `help(AristonHandler)` on how to properly initiate API.

### AquaAristonHandler start communication
```
from aristonremotethermo.aristonaqua import AquaAristonHandler

ApiInstanceAqua = AquaAristonHandler(
            username='username',
            password='password'
        )

ApiInstanceAqua.start()
```
See `help(AquaAristonHandler)` on how to properly initiate API.


### AristonHandler stop communication
```
ApiInstance.stop()
```

### AquaAristonHandler stop communication
```
ApiInstanceAqua.stop()
```

### API properties
See `help(AristonHandler)` and `help(AquaAristonHandler)`.

### AristonHandler change of data on remote server
```
ApiInstance.set_http_data(parameter1=value1,parameter2=value2,...)
```
Method sets values for specific parameter names (see property `supported_sensors_set_values` from `help(AristonHandler)`) on the remote server.

#### AristonHandler change of data example
```
ApiInstance.set_http_data(mode="winter",ch_mode="scheduled")
```

### AquaAristonHandler change of data on remote server
```
ApiInstanceAqua.set_http_data(parameter1=value1,parameter2=value2,...)
```
Method sets values for specific parameter names (see property `supported_sensors_set_values` from `help(AquaAristonHandler)`) on the remote server.

#### AquaAristonHandler change of data example
```
ApiInstanceAqua.set_http_data(mode="manual")
```
