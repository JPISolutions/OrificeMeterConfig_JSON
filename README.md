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
		"units": "metric",
		"contractHour": 8,

## AGA3
This section contains all the primary parameters required for the AGA3 calculation. Orifice and pipe materials shall be the materials specified in the AGA3 standard. They will be listed here sometime soon too. 

		"AGA3":  {
			"calculation": "AGA3 1992",
			"orificeTap": "Flange",
			"orificeDiameter": 25.1,
			"orificeMaterial": "316 Stainless Steel",
			"orificeReferenceTemprature": 20,
			"pipeInsideDiameter": 76.1,
			"pipeMaterial": "Carbon Steel",
			"pipeReferenceTemperature": 20,
			"baseTemperature": 15,
			"basePressure": 101.325,
			"isentropicExponent": 1.3,
			"viscosity": 0.010268,
			"flowExtentsion": "Method 1"
		},

## AGA8
The AGA8 section contains the information about the fluid being measured. This information is used by the flow computer to calculate fluid density which is a primary input for the AGA3 mass flow calculation. Values for components will be in percent (%), not molar fraction. 

		"AGA8": {
				"components": [
					{ "component": "H2", "value": 0},
					{ "component": "He", "value": 0},
					{ "component": "CO2", "value": 0},
					{ "component": "H2S", "value": 0},
					{ "component": "C1", "value": 0},
					{ "component": "C2", "value": 0},
					{ "component": "C3", "value": 0},
					{ "component": "iC4", "value": 0},
					{ "component": "nC4", "value": 0},
					{ "component": "iC5", "value": 0},
					{ "component": "nC5", "value": 0},
					{ "component": "C6", "value": 0},
					{ "component": "C7", "value": 0},
					{ "component": "C8", "value": 0},
					{ "component": "C9", "value": 0},
					{ "component": "C10", "value": 0},
					{ "component": "N2", "value": 0},
					{ "component": "H2O", "value": 0},
					{ "component": "CO", "value": 0},
					{ "component": "O2", "value": 0},
					{ "component": "Ar", "value": 0}
				],
				"density": 50.0,
				"compressibility": 0.0
		},
		
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
				"sensorType": "gauge",
				"atmosphericPressure": 101.325				
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
