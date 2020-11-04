# DualUniverse-IndustryCommandCenter
This highly customizable script will allow you to connect containers, industries and screens to allow monitoring and control over the attached industries. This script requires customization for each setup, so may require slightly more technical skills to implement. Some elements of this were adapted from https://github.com/thespartacus29/DualUniverse-OreMonitor, so thanks goes to thespartacus29 for the open code. 

## Change Log
- 11/03/2020 - Initial Load

## Elements required

- 1 Screen per 8 industries/containers
- 1 Programming Board per 8 industries/containers
- 1 Databank per 10 Programming Boards
- As many switches, relays and buttons as necessary 

## Linking Setup

Each Programming Board is configured with the following links:
- 1 "Screen" slot to be connected to any size screen. This is a mandatory component.
- 1 "Databank" slot to be connected to any size screen. This is an optional component and is only used to save settings to allow quick updates. 
- 8 "element" slots. These can be connected to either an industry element such as an Assembly Line or Metalworker, or to a container or container hub. The display will adjust automatically depending on what was connected to it.

## Setup Parameters
All modifiable params are at the top of unit.start() and are marked with an --export:.

machineName = "Tier 1 Ore" --export: The product being made
maintainAmount = 375000 --export: The amount that is being maintained
itemMass = 1 --export: The mass per unit/L/M3 of the unit being produced in kg. This value is used as the default unless a specific slot override is provided. 

headerSize = "8vw" --export: The size of the header font. Decrease for larger names.
fontColor = "black" --export: The color of the text
backgroundColor = "white" --export: The normal background color to use, mainly for command tiles.
okColor = "#98FB98" --export: The background color to use when everything is good
warningColor = "#FFFF00" --export: The background color to use for warnings
errorColor= "#FF4500" --export: The background color to use when an error occurs

efficiencyWarn = 90 --export: If efficiency is greater than this, a warning will be displayed. Use 101 to disable. 
overstockWarningAmount = 150 --export: If the current stock is greater than this amount (in %), a warning will be displayed. Applies to all containers.
overstockErrorAmount = 200 --export: If the current stock is greater than this amount (in %), an error will be displayed. Applies to all containers.

element1Labell= "Bauxite:<br/>" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label2= "Bauxite:<br/>" --export: Can be used to provide additional information on the table output for Machine 2.
element1Label3= "Coal:<br/>" --export: Can be used to provide additional information on the table output for Machine 3.
element1Label4= "Coal:<br/>" --export: Can be used to provide additional information on the table output for Machine 4.
element1Label5= "Hematite:<br/>" --export: Can be used to provide additional information on the table output for Machine 5.
element1Label6= "Hematite:<br/>" --export: Can be used to provide additional information on the table output for Machine 6.
element1Label7= "Quartz:<br/>" --export: Can be used to provide additional information on the table output for Machine 7.
element1Label8= "Quartz:<br/>" --export: Can be used to provide additional information on the table output for Machine 8.

loadFromDatabank="True" --export: Used to load from databank values instead of these values. This value will always be loaded from the databank, so save after converting to load from config in the UI. If no value for a key is found in the databank, it will default to the value provided in the config. 

itemMassOverride1=1.2808  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride2=1.2808  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride3=1.34545  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride4=1.34545  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride5=5.04  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride6=5.04  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride7=2.65  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride8=2.65  --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 



## Screen Controls
All controls except for the "Loading From: X" provide interactive feedback in the save bar. When feedback is being presented, all controls are disabled. This will automatically clear after 5 seconds. 

Save Configuration
This is used to save the current setup parameters to the connected databank. This will fail if there is no databank is connected. To update the saved settings, switch to "Loading From: Config" and enter new values in the script. Once loaded, save again to commit the new values to the databank. All keys are appended with the unique machine Id, so multiple machines can share the same databank.

Loading From: X
This control is used to change where values are loaded from. Clicking on it will toggle back and forth between "Databank" and "Config". Loading values from the databank is the default and is useful so that scripts can be upgraded. 

Start
Pushing this control will start all machines in a "STOPPED" state. If the maintain amount is set to 0, it will be set to Run mode, otherwise it will start with the maintain amount provided in the config.

Stop
Sends a "Finish and Stop" command to all attached machines. 

Restart
Sends a hard stop to all machines, and then restarts them. It uses a safe hard stop though, and will not stop machines if it will cause the loss of materials. 




## Sample Setup - Pure Carbon
This sample setup is used to produce Pure Carbon. You will need 1 screen, 1 databank, 1 container/container hub, and 1 or more refiners. You can control the industries entirely from the connected screen, so update the maintain amounts in the config and use the "Start", "Stop" and "Restart" buttons to issue commands to all attached machines at once. Additionally, warnings and errors will be presented under the following circumstances:
- warning for low ore (less than 50%)
- error for really low ore (less than 25%)
- warning for high efficiency (over 90%, but can be adjusted below). This is useful for determining bottle necks in production as I will usually add a machine if all machines are running over 90%. 
- error if a machine in a state other than "RUNNING" or "PENDING". The specific error is displayed on the screen and can be used to troubleshoot. 
- warning for "Overstock" amount. This may happen if too much by product builds up in the bin. 
- error for "Overstock" amount. This may happen if way too much by product builds up in the bin. 

Machine Connections:
Screen - To an XS Screen
Databank - To any databank
Element1 - To a container or container hub holding the pure carbon
Element2-8 - To refiners that have the "Pure Carbon" recipe applied. 

Startup Parameters
machineName = "Pure Carbon" --export: The product being made
maintainAmount = 1000 --export: The amount that is being maintained
itemMass = 2.27 --export: The mass per unit/L/M3 of the unit being produced in kg

headerSize = "8vw" --export: The size of the header font. Decrease for larger items.
fontColor = "black" --export: The color of the text
backgroundColor = "white" --export: The normal background color to use.
okColor = "#98FB98" --export: The background color to use when everything is good
warningColor = "#FFFF00" --export: The background color to use for warnings
errorColor= "#FF4500" --export: The background color to use when an error occurs

efficiencyWarn = 90 --export: If efficiency is greater than this, a warning will be displayed. Use 101 to disable. 
overstockWarningAmount = 125 --export: If the current stock is greater than this amount (in %), a warning will be displayed.
overstockErrorAmount = 150 --export: If the current stock is greater than this amount (in %), an error will be displayed.

element1Labell= "" --export: Can be used to provide additional information on the table output for the attached machine.
element1Label2= "" --export: Can be used to provide additional information on the table output for the attached machine.
element1Label3= "" --export: Can be used to provide additional information on the table output for the attached machine.
element1Label4= "" --export: Can be used to provide additional information on the table output for the attached machine.
element1Label5= "" --export: Can be used to provide additional information on the table output for the attached machine.
element1Label6= "" --export: Can be used to provide additional information on the table output for the attached machine.
element1Label7= "" --export: Can be used to provide additional information on the table output for the attached machine.
element1Label8= "" --export: Can be used to provide additional information on the table output for the attached machine.

loadFromDatabank="True" --export: Used to load from databank values instead of these values. If no values are found, these values will be used instead.

itemMassOverride1=0
itemMassOverride2=0
itemMassOverride3=0
itemMassOverride4=0
itemMassOverride5=0
itemMassOverride6=0
itemMassOverride7=0
itemMassOverride8=0



## Sample Setup - Pure Carbon TU
This sample setup is used to create additional links for Pure Carbon to be used for other industries. You will need 1 screen, 1 databank, 1-4 container/container hubs, and 1-4 transfer units. You can control the industries entirely from the connected screen, so update the maintain amounts in the config and use the "Start", "Stop" and "Restart" buttons to issue commands to all attached machines at once. Additionally, warnings and errors will be presented under the following circumstances:
- warning for low ore (less than 50%) on any of the attached containers.
- error for really low ore (less than 25%) on any of the attached containers.
- warning for high efficiency (over 90%, but can be adjusted below).
- error if a machine in a state other than "RUNNING" or "PENDING". The specific error is displayed on the screen and can be used to troubleshoot. 

Machine Connections:
Screen - To an XS Screen
Databank - To any databank
Element1,3,5,8 - To a container or container hub holding the pure carbon
Element2,4,6,8 - To refiners that have the "Pure Carbon" recipe applied. 

Startup Parameters
machineName = "Pure Carbon TU" --export: The product being made
maintainAmount = 500 --export: The amount that is being maintained
itemMass = 2.27 --export: The mass per unit/L/M3 of the unit being produced in kg

headerSize = "7vw" --export: The size of the header font. Decrease for larger items.
fontColor = "black" --export: The color of the text
backgroundColor = "white" --export: The normal background color to use.
okColor = "#98FB98" --export: The background color to use when everything is good
warningColor = "#FFFF00" --export: The background color to use for warnings
errorColor= "#FF4500" --export: The background color to use when an error occurs

efficiencyWarn = 90 --export: If efficiency is greater than this, a warning will be displayed. Use 101 to disable. 
overstockWarningAmount = 10000 --export: If the current stock is greater than this amount (in %), a warning will be displayed.
overstockErrorAmount = 20000 --export: If the current stock is greater than this amount (in %), an error will be displayed.

element1Labell= "" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label2= "" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label3= "" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label4= "" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label5= "" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label6= "" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label7= "" --export: Can be used to provide additional information on the table output for Machine 1.
element1Label8= "" --export: Can be used to provide additional information on the table output for Machine 1.

loadFromDatabank="True" --export: Used to load from databank values instead of these values. If no values are found, these values will be used instead.

itemMassOverride1=0
itemMassOverride2=0
itemMassOverride3=0
itemMassOverride4=0
itemMassOverride5=0
itemMassOverride6=0
itemMassOverride7=0
itemMassOverride8=0

Alternative Config:
Containers could be considered optional here, so you could run up to 8 Transfer Units per programming board if you opt out of monitoring containers. 






## Sample Setup - Pure Carbon TU
This sample setup is used to remove by products from pure and product bins. You will need 1 screen, 1 databank, 1-8 transfer units. You can control the industries entirely from the connected screen, so update the maintain amounts in the config and use the "Start", "Stop" and "Restart" buttons to issue commands to all attached machines at once. Errors are disabled in this mode, so you are unlikely to see any other status than green.

Machine Connections:
Screen - To an XS Screen
Databank - To any databank
Element1-8 - To a transfer unit connected to a pure or product bin set to a recipe to remove by products. For example, removing oxygen from Carbon and Iron pures, or removing catalyst 3 from any T3 product. 

Startup Parameters
machineName = "Clearing TU" --export: The product being made
maintainAmount = 0 --export: The amount that is being maintained
itemMass = 1 --export: The mass per unit/L/M3 of the unit being produced in kg

headerSize = "7vw" --export: The size of the header font. Decrease for larger items.
fontColor = "black" --export: The color of the text
backgroundColor = "white" --export: The normal background color to use.
okColor = "#98FB98" --export: The background color to use when everything is good
warningColor = "#FFFF00" --export: The background color to use for warnings
errorColor= "#FF4500" --export: The background color to use when an error occurs

efficiencyWarn = 90 --export: If efficiency is greater than this, a warning will be displayed. Use 101 to disable. 
overstockWarningAmount = 10000 --export: If the current stock is greater than this amount (in %), a warning will be displayed.
overstockErrorAmount = 20000 --export: If the current stock is greater than this amount (in %), an error will be displayed.

element1Labell= "Pure Hydrogen<br/>" --export: Can be used to provide additional information on the table output for for the attached machine.
element1Label2= "Pure Oxygen<br/>" --export: Can be used to provide additional information on the table output for for the attached machine.
element1Label3= "" --export: Can be used to provide additional information on the table output for for the attached machine.
element1Label4= "" --export: Can be used to provide additional information on the table output for for the attached machine.
element1Label5= "" --export: Can be used to provide additional information on the table output for for the attached machine.
element1Label6= "" --export: Can be used to provide additional information on the table output for for the attached machine.
element1Label7= "" --export: Can be used to provide additional information on the table output for for the attached machine.
element1Label8= "" --export: Can be used to provide additional information on the table output for for the attached machine.

loadFromDatabank="True" --export: Used to load from databank values instead of these values. If no values are found, these values will be used instead.

itemMassOverride1=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride2=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride3=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride4=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride5=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride6=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride7=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 
itemMassOverride8=0 --export: Used to override the specific mass value for the attached container/machine. Useful for ore monitors. 

machineFailureOverride=1 --Use this to to disable errors on industry machines that are erroring. Useful for Transfer Units that remove byproducts.

