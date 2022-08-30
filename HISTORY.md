# Version "2.0.12"
- INCOMPATIBLE CHANGES due to zones introduction. Input sensors are the same but the structure inside for zone sensors include additional "_zone1", "_zone2" ...;

# Version "2.0.11"
- Some unknown energy use for DHW and CH (e.g. on some models that support 6 parameters "ch_energy + ch_energy_delta == ch_energy2" );

# Version "2.0.10"
- Additional set of energy sensors (some models have both some have one);

# Version "2.0.9"
- energy use fix;

# Version "2.0.7"
- dhw flame fix;

# Version "2.0.7"
- energy consumption sensors update;
- code refactoring;

# Version "2.0.5"
- todayes energy consumption calculation;

# Version "2.0.4"
- new energy use sensors;
- bug fixes

# Version "2.0.1"
- multiple incompatible changes, reduced amount of supported functions due to changes in REST requests by Ariston;

# Version "1.0.52"
- login fix;

# Version "1.0.51"
- new sensors support;

# Version "1.0.49"
- request handling update;

# Version "1.0.48"
- Todays energy use sensor handling update;

# Version "1.0.47"
- Multiple zones support;

# Version "1.0.46"
- Revision align;

# Version "1.0.44"
- Updated handling of parameter sensors;
- BREAKING CHANGE: `AquaAristonHandler` moved to 'https://pypi.org/project/aristonremotethermo/'.

# Version "1.0.43"
- New sensors in Ariston integration for energy consumption today;

# Version "1.0.41"
- Dedicated files per gateway;

# Version "1.0.40"
- Multiple boilers under one account (gateway must be specified per each instance);

# Version "1.0.38"
- Unsupported sensor detection attempt in Ariston integration;

# Version "1.0.37"
- Cooling included in Ariston integration;

# Version "1.0.35"
- Bug fixes;

# Version "1.0.34"
- Economy temperature reporting in set values when economy mode is active;

# Version "1.0.33"
- Read Plant ID property method included;

# Version "1.0.32"
- Tweaks in supported parameters detection to avoid issues on specific configurations;

# Version "1.0.31"
- Class method for reading the data is included;

# Version "1.0.30"
- Callback function added to inform about values received from the server or status changed;

# Version "1.0.29"
- Logging to console included;
- Lydos Hybrid maximum value update;

# Version "1.0.28"
- CH Water temperature added in AristonHandler for supported models;

# Version "1.0.27"
- Additional data stored in case of server errors;

# Version "1.0.26"
- Fix error parsing for AristonHandler;

# Version "1.0.25"
- Fix units of measurement setting;

# Version "1.0.24"
- Availability handling update;

# Version "1.0.23"
- Availability handling update;

# Version "1.0.22"
- Night mode added;

# Version "1.0.21"
- Fixing interation with Alteas One;
- CRITICAL: manual identification of Aqua Ariston boiler is required due to problem with automated identification;

# Version "1.0.20"
- Lydos Hybrid included;

# Version "1.0.19"
- Ariston added cooling pump statistics;

# Version "1.0.18"
- Aqua Ariston fix issue with no sensors being provided;

# Version "1.0.17"
- Storing of files fixed;
- New handling of required temperatures for Velis wifi;

# Version "1.0.16"
- Setting of temperature modified for Velis Wifi;

# Version "1.0.15"
- Setting of data fixed for AquaAristonHandler;

# Version "1.0.14"
- Code cleanup;

# Version "1.0.13"
- Code cleanup;
- Additional class for handling Velis is introduced;

# Version "1.0.12"
- DHW and CH availability status update;

# Version "1.0.11"
- Code cleanup;

# Version "1.0.10"
- Folder creation handling for data store is updated;

# Version "1.0.9"
- Change the way changed temperature is visualized;

# Version "1.0.8"
- Change the way floats are compared in all places;
- More flexible data types are allowed in set_http_data method;

# Version "1.0.7"
- Help annotations update;

# Version "1.0.6"
- Changed algorithm to visualize temperatures when changing their values;

# Version "1.0.5"
- Help update;

# Version "1.0.4"
- Initial release;