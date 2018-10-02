# OrificeMeterConfig_JSON
An open source data format for an orifice meter using the AGA3 / AGA8 standard. 
This JSON format is intended for data storage and transfer of a EFM configuration between cloud services and EFM configuration applications. 

# Basic Format
## Configuration
This section contains the primary identifying and location information for this meter. Units shall be "imperial" or "meter". The units of parameters through the rest of the configuration file shall be the units as specified by the AGA3 standard.  

	"configuration": {
		"name": "meterName",
		"owner": "ownerName",
		"latitude": 123.456,
		"longitude": 123.456,
		"elevation": 0.0,
		"baseTemperature": 15,
		"basePressure": 101.325,
		"atmosphericPressure": 101.325,
		"units": "Metric",
		"contractHour": 8,

### Configuration - Fields
Question for contributors: Do we need more descriptive metadata?
* name - String field containing the name of the meter
* owner - String field indicating the owner of the meter
* latitude, longitude, elevation - Location information
* baseTemperature - Temperature at 'standard' conditions. ISO Standard: Metric = 15degC. Imperial = 59degF. US Standard: Metric = 15.56degC. Imperial 60degF. 
* basePressure - Pressure at 'standard' conditions. Metric = 101.325kPa. Imperial = 14.696psi.
* units - Metric or Imperial.
	* Metric
		* Volume = E3m3/day
		* Pressure = kPa
		* Temperature = degC
	* Imperial
		* Volume = MCF
		* Differential Pressure = inH2O
		* Static Pressure = psi
		* Temperature degF
* contractHour - this is the meter's daily roll over time, also known as Contract Hour. Often defined by the custody transfer contract. 

## AGA3
This section contains all the primary parameters required for the AGA3 calculation. Orifice and pipe materials shall be the materials specified in the AGA3 standard. They will be listed here sometime soon too. 
Edit 2018/10/02 - Moved baseTemperature and basePressure up a level since it may be used with the AGA8 parameters as well. 

		"AGA3":  {
			"calculation": "AGA3 1992",
			"orificeTap": "Flange",
			"orificeDiameter": 25.1,
			"orificeMaterial": "316 Stainless Steel",
			"orificeReferenceTemperature": 20,
			"pipeInsideDiameter": 76.1,
			"pipeMaterial": "Carbon Steel",
			"pipeReferenceTemperature": 20,
			"isentropicExponent": 1.3,
			"viscosity": 0.010268,
			"flowExtentsion": "Method 1"
		},
		
### AGA3 - Fields
Contributors please update valid options if you know they are different. 
* calculation - This specifies which version of the AGA3 calculation this meter is using. Valid options:
	* AGA3_1985
	* AGA3_1992
	* AGA3_2013
* orificeTap - Flange or ??
* orificeDiameter - The diameter of the orifice hole in the orifice plate. Metric = mm. Imperial = inches. 
* orificeMaterial - material the orifice plate is made of. See below for valid options. 
* orificeReferenceTemperature - the temperature at which the orifice diameter was measured. Metric = Deg C. Imperial = Deg F. 
* pipeInsideDiameter - The internal diameter of the meter tube. AKA meter tube internal diameter. Must be stamped on the meter tube itself. Metric = mm. Imperial = inches. 
* pipeMaterial - The material the meter tube is made out of. 
* pipeReferenceTemperature - the temperature at which the pipe diameter was measure. Metric = Deg C. Imperial = Deg F. 
* Valid orifice and pipe materials:
	* Type_304/316_Stainless_Steel
	* Type_304_Stainless_Steel
	* Type_316_Stainless_Steel
	* Monel_400
	* Carbon_Steel
	
	These materials provide the linear coefficient of thermal expansion (available in the AGA3 standard). That coefficient is only good for certain flowing temperatures, as noted in the standard. 
pipeReferenceTemperature - The temperature at which the inside diameter was measured.


## AGA8
The AGA8 section contains the information about the fluid being measured. This information is used by the flow computer to calculate fluid density which is a primary input for the AGA3 mass flow calculation. Values for components will be in percent (%), not molar fraction. 

		"AGA8": {
				"units": "molar_fraction",
				"components": [
					{ "component": "H2", "value": 0.0},
					{ "component": "He", "value": 0.0},
					{ "component": "CO2", "value": 0.0},
					{ "component": "H2S", "value": 0.0},
					{ "component": "C1", "value": 0.0},
					{ "component": "C2", "value": 0.0},
					{ "component": "C3", "value": 0.0},
					{ "component": "iC4", "value": 0.0},
					{ "component": "nC4", "value": 0.0},
					{ "component": "iC5", "value": 0.0},
					{ "component": "nC5", "value": 0.0},
					{ "component": "C6", "value": 0.0},
					{ "component": "C7", "value": 0.0},
					{ "component": "C8", "value": 0.0},
					{ "component": "C9", "value": 0.0},
					{ "component": "C10", "value": 0.0},
					{ "component": "N2", "value": 0.0},
					{ "component": "H2O", "value": 0.0},
					{ "component": "CO", "value": 0.0},
					{ "component": "O2", "value": 0.0},
					{ "component": "Ar", "value": 0.0}
				],
				"density": 50.0,
				"compressibility": 0.0
		},
		
### AGA8 Fields
* units - Valid options are molar_fraction or percent. 
* components - An array of components that make up the gas composition. Components may be missing from the array if not in the gas stream.
* density - This is the density of the gas stream. Usually calculated live on the fly in modern RTUs and flow computers. May be configured in some flow computers though. 
* compressibility - This is the compressibility of the gas stream at base conditions. Usually calculated live on the fly in modern RTUs and flow computers. May be configured in some flow computers though.

## Sensors
Information about the sensing device(s) is contained here. Source types shall be "multi-variable" or "single". Serial numbers are optional but are recommended to be included. 

		"sensors": {
			"differentialPressure": {
				"source": "multi-variable",
				"serialNumber": "SN_TEXT",
				"minimumRange": 0.0,
				"maximumRange": 100.0,
				"units": "kPa"
			},
			"staticPressure": {
				"source": "multi-variable",
				"serialNumber": "SN_TEXT",
				"minimumRange": 0.0,
				"maximumRange": 10000.0,
				"units": "kPa",
				"tapLocation": "upstream",
				"sensorType": "gauge"			
			},
			"flowingTemperature": {
				"source": "multi-variable",
				"serialNumber": "SN_TEXT",
				"minimumRange": -40.0,
				"maximumRange": 100.0,
				"units": "Celsius"
			}
		}
	}
}
