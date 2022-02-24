# Switch Installation  [Cisco Catalyst 2960-X Series Switches] - Cisco Systems
Contents

-   **[Switch Installation](#topic_4B4E1482A93E45D2A3A01AA2E5D37ED8)**
-   [Safety Warnings](#ID8)
-   [Box Contents](#ID104)
-   [Tools and Equipment](#ID106)
-   [Installation Guidelines](#ID74)
-   [Verifying Switch Operation](#ID112)
-   [Planning and Installing a Switch Stack (Optional)](https://www.cisco.com/en/US/docs/switches/lan/catalyst2960x/hardware/installation/guide/b_c2960x_hig_chapter_010.html#ID126)
-   [Stack Guidelines](#ID129)
-   [Installing the FlexStack-Plus Module](#task_86B7D99A817D45B48923D25B65CEA1CD)
-   [Stack Cabling](#ID173)
-   [Stack Bandwidth and Partitioning Examples](#ID187)
-   [Power-On Sequence for Switch Stacks](#ID216)
-   [Installing the Switch](#ID227)
-   [Rack-Mounting](#ID240)
-   [Attaching the Rack-Mount Brackets for the Catalyst 2960-X Switches](#ID254)
-   [Mounting in a Rack](#ID284)
-   [Wall-Mounting](#ID314)
-   [Attaching the Brackets for Wall-Mounting](#ID327)
-   [Attaching the RPS Connector Cover](#ID341)
-   [Mounting on a Wall](#ID364)
-   [Installing the Switch on a Table or Shelf](#ID377)
-   [After Switch Installation](#ID388)
-   [Connecting the FlexStack Cables (Optional)](#ID406)
-   [Removing a FlexStack Cable](#ID428)
-   [Installing the Power Cord Retainer (Optional)](#ID436)
-   [Installing SFP and SFP+ Modules](#ID518)
-   [Installing an SFP or SFP+ Module](#ID526)
-   [Removing an SFP or SFP+ Module](#ID555)
-   [Connecting to SFP and SFP+ Modules](#ID569)
-   [Connecting to Fiber-Optic SFP and SFP+ Modules](#ID575)
-   [Connecting to 1000BASE-T SFP](#ID610)
-   [10/100/1000 PoE+ Port Connections](#ID638)
-   [10/100/1000 Port Connections](#ID700)
-   [Auto-MDIX Connections](#ID669)
-   [Where to Go Next](#ID710)

Switch Installation

* * *

============================

For initial switch setup, assigning the switch IP address, and powering on information, see the switch getting started guide on Cisco.com.

This chapter contains these topics:

-   [Safety Warnings](#ID8)
-   [Box Contents](#ID104)
-   [Tools and Equipment](https://www.cisco.com/en/US/docs/switches/lan/catalyst2960x/hardware/installation/guide/b_c2960x_hig_chapter_010.html#ID106)
-   [Installation Guidelines](#ID74)
-   [Verifying Switch Operation](#ID112)
-   [Planning and Installing a Switch Stack (Optional)](#ID126)
-   [Installing the Switch](#ID227)
-   [Connecting the FlexStack Cables (Optional)](#ID406)
-   [Installing the Power Cord Retainer (Optional)](#ID436)
-   [Installing SFP and SFP+ Modules](#ID518)
-   [Connecting to SFP and SFP+ Modules](https://www.cisco.com/en/US/docs/switches/lan/catalyst2960x/hardware/installation/guide/b_c2960x_hig_chapter_010.html#ID569)
-   [10/100/1000 PoE+ Port Connections](#ID638)
-   [10/100/1000 Port Connections](#ID700)
-   [Where to Go Next](#ID710)

## Safety Warnings

This section includes the basic installation caution and warning statements. Read this section before you start the installation procedure. Translations of the warning statements appear in the RCSI guide on Cisco.com.

\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Before working on equipment that is connected to power lines, remove jewelry (including rings, necklaces, and watches). Metal objects will heat up when connected to power and ground and can cause serious burns or weld the metal object to the terminals. Statement 43

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Do not stack the chassis on any other equipment. If the chassis falls, it can cause severe bodily injury and equipment damage. Statement 48

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

This product must be connected to a power-over-ethernet (PoE) IEEE 802.3af compliant power source or an IEC60950 compliant limited power source. Statement 353

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Read the wall-mounting instructions carefully before beginning installation. Failure to use the correct hardware or to follow the correct procedures could result in a hazardous situation to people and damage to the system. Statement 378

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Attach only the following Cisco external power system to the switch: PWR-RPS2300 Statement 387

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Do not work on the system or connect or disconnect cables during periods of lightning activity. Statement 1001

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Read the installation instructions before connecting the system to the power source. Statement 1004

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

To prevent bodily injury when mounting or servicing this unit in a rack, you must take special precautions to ensure that the system remains stable. The following guidelines are provided to ensure your safety: 

-   This unit should be mounted at the bottom of the rack if it is the only unit in the rack.

-   When mounting this unit in a partially filled rack, load the rack from the bottom to the top with the heaviest component at the bottom of the rack.

-   If the rack is provided with stabilizing devices, install the stabilizers before mounting or servicing the unit in the rack.

Statement 1006

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Class 1 laser product. Statement 1008

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

This unit is intended for installation in restricted access areas. A restricted access area can be accessed only through the use of a special tool, lock and key, or other means of security. Statement 1017

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

The plug-socket combination must be accessible at all times, because it serves as the main disconnecting device. Statement 1019

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

This equipment must be grounded. Never defeat the ground conductor or operate the equipment in the absence of a suitably installed ground conductor. Contact the appropriate electrical inspection authority or an electrician if you are uncertain that suitable grounding is available. Statement 1024

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

This unit might have more than one power supply connection. All connections must be removed to de-energize the unit. Statement 1028

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Only trained and qualified personnel should be allowed to install, replace, or service this equipment. Statement 1030

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Ultimate disposal of this product should be handled according to all national laws and regulations. Statement 1040

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

For connections outside the building where the equipment is installed, the following ports must be connected through an approved network termination unit with integral circuit protection: 10/100/1000 Ethernet. Statement 1044

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

When installing or replacing the unit, the ground connection must always be made first and disconnected last. Statement 1046

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

To prevent the system from overheating, do not operate it in an area that exceeds the maximum recommended ambient temperature of: &lt;113°F (45°C). Statement 1047

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

This warning symbol means danger. You are in a situation that could cause bodily injury. Before you work on any equipment, be aware of the hazards involved with electrical circuitry and be familiar with standard practices for preventing accidents. Use the statement number provided at the end of each warning to locate its translation in the translated safety warnings that accompanied this device. Statement 1071

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Voltages that present a shock hazard may exist on Power over Ethernet (PoE) circuits if interconnections are made using uninsulated exposed metal contacts, conductors, or terminals. Avoid using such interconnection methods, unless the exposed metal parts are located within a restricted access location and users and service people who are authorized within the restricted access location are made aware of the hazard. A restricted access area can be accessed only through the use of a special tool, lock and key or other means of security. Statement 1072

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

No user-serviceable parts inside. Do not open. Statement 1073

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Installation of the equipment must comply with local and national electrical codes. Statement 1074

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

To prevent airflow restriction, allow clearance around the ventilation openings to be at least: 3 inches (7.6 cm). Statement 1076

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Hot surface. Statement 1079

* * *

 \|

## Box Contents

The switch getting started guide describes the box contents. If any item is missing or damaged, contact your Cisco representative or reseller for support.

## Tools and Equipment

Obtain these necessary tools and equipment:

-   A number-2 Phillips screwdriver to rack-mount the switch.

## Installation Guidelines

When determining where to install the switch, verify that these guidelines are met:

-   Clearance to the switch front and rear panel meets these conditions:
    -   Front-panel LEDs can be easily read.
    -   Access to ports is sufficient for unrestricted cabling.
    -   AC power cord can reach from the AC power outlet to the connector on the switch rear panel.
    -   Access to the rear of the rack is sufficient for connecting FlexStack cables to stacked switches, or connecting the optional Cisco Redundant Power Supply (RPS) 2300.
-   Cabling is away from sources of electrical noise, such as radios, power lines, and fluorescent lighting fixtures. Make sure that the cabling is safely away from other devices that might damage the cables.
-   Airflow around the switch and through the vents is unrestricted.
-   Temperature around the unit does not exceed 113°F (45°C). If the switch is installed in a closed or multirack assembly, the temperature around it might be greater than normal room temperature.
-   Humidity around the switch does not exceed 95 percent.
-   Altitude at the installation site is not greater than 10,000 feet.
-   For 10/100/1000 fixed ports, the cable length from a switch to a connected device cannot exceed 328 feet (100 meters).
-   Cooling mechanisms, such as fans and blowers in the switch, can draw dust and other particles causing contaminant buildup inside the chassis, which can result in system malfunction. You must install this equipment in an environment as free from dust and foreign conductive material (such as metal flakes from construction activities) as is possible.

## Verifying Switch Operation

Before you install the switch in a rack, on a wall, or on a table or shelf, power on the switch and verify that it passes POST.

To power on the switch, plug one end of the AC power cord into the switch AC power connector, and plug the other end into an AC power outlet.

As the switch powers on, it begins the POST, a series of tests that runs automatically to ensure that the switch functions properly. LEDs can blink during the test. POST lasts approximately 1 minute. When the switch begins POST, the SYST, RPS, STAT, and SPEED LEDs turn green. The SYST LED blinks green, and the other LEDs remain solid green.

When the switch completes POST successfully, the SYST LED remains green. The RPS LED remains green for some time and then reflects the switch operating status. The other LEDs turn off and then reflect the switch operating status. If a switch fails POST, the SYST LED turns amber.

POST failures are usually fatal. Call Cisco technical support representative if your switch fails POST.

After a successful POST, unplug the power cord from the switch and install the switch in a rack, on a wall, on a table, or on a shelf.

If your configuration has an RPS, connect the switch and the RPS to different AC power sources. See the Cisco RPS documentation for information.

\| ![](https://www.cisco.com/en/US/i/templates/note.gif)

**Note** \|   

* * *

When you connect the RPS to the switch, put the RPS in standby mode. Set the RPS to active mode during normal operation.

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Attach only the following Cisco external power system to the switch: PWR-RPS2300 Statement 387

* * *

 \|

## Planning and Installing a Switch Stack (Optional)

\| ![](https://www.cisco.com/en/US/i/templates/note.gif)

**Note** \|   

* * *

This section applies only to the Catalyst 2960-X stacking-capable switches.

* * *

 \|

-   [Stack Guidelines](#ID129)
-   [Installing the FlexStack-Plus Module](#task_86B7D99A817D45B48923D25B65CEA1CD)
-   [Stack Cabling](#ID173)
-   [Stack Bandwidth and Partitioning Examples](#ID187)
-   [Power-On Sequence for Switch Stacks](#ID216)

### Stack Guidelines

-   Connect only Catalyst 2960-X or 2960-S switches in a mixed switch stack.
    \| ![](https://www.cisco.com/en/US/i/templates/note.gif)

    **Note** \|   

    * * *

    You can only create mixed stacks with Catalyst 2960-X or 2960-S switches (up to four switches). You cannot create mixed stacks with other switches.

    * * *

     \|
-   Install the FlexStack-Plus module and the FlexStack cable.
    \| ![](https://www.cisco.com/en/US/i/templates/note.gif)

    **Note** \|   

    * * *

    The FlexStack-Plus module is hot-swappable and can be inserted while the switch is powered on.

    * * *

     \|
-   Order the appropriate cable from your Cisco sales representative. The length of FlexStack cable depends on your configuration. These are the different sizes available:
    -   CAB-STK-E-0.5M= (0.5-meter cable)
    -   CAB-STK-E-1M= (1-meter cable)
    -   CAB-STK-E-3M= (3-meter cable)
-   Make sure that you have access to the switch rear panel and to the rear of the rack.

### Installing the FlexStack-Plus Module

\| ![](https://www.cisco.com/en/US/i/templates/note.gif)

**Note** \|   

* * *

The switch should always have a blank module installed when a FlexStack-Plus module is not used.

* * *

 \|

The Catalyst 2960X-48P-L switch is shown as an example. You can install the module in other switches as shown.

**Procedure**

* * *

\| **Step 1**   | Use a number 2 Phillips-head screwdriver to remove the FlexStack-Plus module blank cover on the switch back panel.

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346521.jpg)

 \|
\| **Step 2**   | Grasp the FlexStack-Plus module on the sides, and insert it into the module slot. Push the module in completely until you feel it snap into place.

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346522.jpg)

 \|
\| **Step 3**   | Secure the screws on each side of the module.

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346523.jpg)
\| **Note** \| 

Avoid overtightening the screws.

 \|

 \|

* * *

### Stack Cabling

Figure 1. Stacking Switches with the 0.5-meter FlexStack Cable. These figures show the switches stacked in a vertical rack or on a table. The connections are redundant.  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346524.jpg)

Figure 2. Stacking Switches with 0.5-meter and 3-meter FlexStack Cables  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346525.jpg)

### Stack Bandwidth and Partitioning Examples

Figure 3. Stack with Full Bandwidth Connections. This figure shows a stack that provides full bandwidth with redundant connections.  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346526.jpg)

Figure 4. Stack with Half Bandwidth Connections. This figure shows a stack with incomplete stack cabling connections. This stack provides only half bandwidth and does not have redundant connections.  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346527.jpg)

Figure 5. Stack with a Failover Condition. This figure shows a stack with a bad FlexStack cable in link B. This stack provides only half bandwidth and does not have redundant connections.  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346528.jpg)

Figure 6. Partitioned Stack with a Failover Condition. This figure shows a stack with a bad link B. This stack partitions into two stacks, and switch 1 and switch 3 are stack masters.  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346529.jpg)

### Power-On Sequence for Switch Stacks

Consider these guidelines before you power on the switches in a stack:

-   The sequence in which the switches are first powered on might affect the switch that becomes the stack master.
-   If you want a particular switch to be the stack master, power on that switch first. This switch becomes the stack master and remains the stack master until a master reelection. After 2 minutes, power on the other stack switches.
-   If you have no preference as to which switch becomes the stack master, power on all the switches in the stack within a 1-minute timeframe. These switches participate in the stack master election. Switches powered on after the 1-minute timeframe do not participate in the election.
-   Power off a switch before you add it to or remove it from an existing switch stack.

For conditions that can cause a stack master reelection or to manually elect the stack master, see the Catalyst 2960-X Switch Stacking Configuration Guide on Cisco.com.

## Installing the Switch

### Rack-Mounting

Installation in other than 19-inch racks requires a bracket kit not included with the switch.

\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

To prevent bodily injury when mounting or servicing this unit in a rack, you must take special precautions to ensure that the system remains stable. The following guidelines are provided to ensure your safety: 

-   This unit should be mounted at the bottom of the rack if it is the only unit in the rack.

-   When mounting this unit in a partially filled rack, load the rack from the bottom to the top with the heaviest component at the bottom of the rack.

-   If the rack is provided with stabilizing devices, install the stabilizers before mounting or servicing the unit in the rack.

Statement 1006

* * *

 \|

Figure 7. Rack-Mounting Brackets. This figure shows the standard 19-inch brackets and other optional mounting brackets. You can order the optional brackets from your Cisco sales representative.  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346760.jpg)

\| 

1

 \| 

19-inch brackets

 \| 

3

 \| 

23-inch brackets

 \|
\| 

2

 \| 

ETSI brackets

 \| 

4

 \| 

24-inch brackets

 \|

-   [Attaching the Rack-Mount Brackets for the Catalyst 2960-X Switches](https://www.cisco.com/en/US/docs/switches/lan/catalyst2960x/hardware/installation/guide/b_c2960x_hig_chapter_010.html#ID254)
-   [Mounting in a Rack](#ID284)

#### Attaching the Rack-Mount Brackets for the Catalyst 2960-X Switches

**Procedure**
\| 

Use two Phillips flat-head screws to attach the long side of the bracket to each side of the switch.

Figure 8. Attaching Brackets for 19-inch Racks  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346532.jpg)

\| 

1

 \| 

Front-mounting position

 \| 

3

 \| 

Mid-mounting position

 \|
\| 

2

 \| 

Number-8 Phillips flat-head screws (48-2927-01)

 \| 

4

 \| 

Rear-mounting position

 \|

 \|

#### Mounting in a Rack

**Procedure**

* * *

\| **Step 1**   | Use the four supplied Phillips machine screws to attach the brackets to the rack. |
\| **Step 2**   | Use the black Phillips machine screw to attach the cable guide to the left or right bracket.

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346533.jpg)

\| 

1

 \| 

Cable guide

 \| 

4

 \| 

Number-12 Phillips pan-head screws (48-0523-01) or Number-10 Phillips pan-head screws (48-0627-01)

 \|
\| 

2

 \| 

Phillips machine screw, black (48-0654-01)

 \| 

5

 \| 

Mid-mounting position

 \|
\| 

3

 \| 

Front-mounting position

 \| 

6

 \| 

Rear-mounting position

 \|

 \|

* * *

### Wall-Mounting

\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Read the wall-mounting instructions carefully before beginning installation. Failure to use the correct hardware or to follow the correct procedures could result in a hazardous situation to people and damage to the system. Statement 378

* * *

 \|

-   [Attaching the Brackets for Wall-Mounting](#ID327)
-   [Attaching the RPS Connector Cover](#ID341)
-   [Mounting on a Wall](#ID364)

#### Attaching the Brackets for Wall-Mounting

**Procedure**

* * *

\| **Step 1**   | Attach a 19-inch bracket to one side of the switch. |
\| **Step 2**   | Follow the same steps to attach the second bracket to the opposite side.

Figure 9. Attaching the 19-inch Brackets for Wall-Mounting  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346652.jpg)

\| 

1

 \| 

Number-8 phillips flat-head screws (48-2927-01)

 \|

 \|

* * *

#### Attaching the RPS Connector Cover

\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

If an RPS is not connected to the switch, install an RPS connector cover on the back of the switch. Statement 265

* * *

 \|

**Procedure**
\| 

If you are not using an RPS with your switch, use the two Phillips pan-head screws to attach the RPS connector cover to the back of the switch.

Figure 10. Attaching the RPS Connector Cover  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346535.jpg)

\| 

1

 \| 

Phillips pan-head screws (48-0482-01)

 \| 

3

 \| 

RPS connector

 \|
\| 

2

 \| 

RPS connector cover

 |   |   |

 \|

#### Mounting on a Wall

For the best support of the switch and cables, make sure that the switch is attached securely to wall studs or to a firmly attached plywood-mounting backboard. Mount the switch with the front panel facing down.

\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Read the wall-mounting instructions carefully before beginning installation. Failure to use the correct hardware or to follow the correct procedures could result in a hazardous situation to people and damage to the system. Statement 378

* * *

 \|

\| ![](https://www.cisco.com/en/US/i/templates/caut.gif)

**Caution** \|   

* * *

Following safety regulations, wall-mount the switch with its front panel facing down.

* * *

 \|

Figure 11. Mounting on a Wall  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346536.jpg)

\| 

1

 \| 

User-supplied screws (for example, you can use # 6 wood screws with a washer head 1-inch long).

 \|

When you complete the switch installation, see [After Switch Installation](#ID388) for information on switch configuration.

### Installing the Switch on a Table or Shelf

**Procedure**

* * *

\| **Step 1**   | To install the switch on a table or shelf, locate the adhesive strip with the rubber feet in the mounting-kit envelope. |
\| **Step 2**   | Attach the four rubber feet to the four circular etches on the bottom of the chassis. |
\| **Step 3**   | Place the switch on the table or shelf near an AC power source. |
\| **Step 4**   | When you complete the switch installation, see the [After Switch Installation](#ID388) for information on switch configuration. |

* * *

### After Switch Installation

-   Configure the switch by running Express Setup to enter the initial switch configuration. See the switch getting started guide on Cisco.com.
-   Use the CLI setup program to enter the initial switch configuration.
-   Connect to the stack ports.
-   Install the power cord retainer (optional).
-   Connect to the front-panel ports.

**Related Tasks**  

[Connecting the FlexStack Cables (Optional)](#ID406)

[Installing the Power Cord Retainer (Optional)](#ID436)

[Installing an SFP or SFP+ Module](#ID526)

[10/100/1000 PoE+ Port Connections](#ID638)

## Connecting the FlexStack Cables (Optional)

Always use a Cisco-approved FlexStack cable to connect the switches.

\| ![](https://www.cisco.com/en/US/i/templates/note.gif)

**Note** \|   

* * *

This is only supported on the stack-capable switches.

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/caut.gif)

**Caution** \|   

* * *

Use only approved cables, and connect only to other Catalyst 2960-X or 2960-S switches. Equipment might be damaged if connected to other nonapproved Cisco cables or equipment.

* * *

 \|

**Procedure**

* * *

\| **Step 1**   | Remove the dust covers from the FlexStack cables, and store them for future use. |
\| **Step 2**   | Insert one end of the FlexStack cable into the stack port of the first switch. Insert the other end of the cable into the stack port on the other switch. Make sure that you insert the cables in completely until you feel them snap into place.

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346537.jpg)

\| **Note** \| 

When you connect the FlexStack cable to the STACK 1 port, the tab should be above the connector. When you connect the FlexStack cable to the STACK 2 port, the tab should be below the connector.

 \| \|
\| **Step 3**   | Replace the dust covers when you remove the FlexStack cables from the connectors.

\| **Caution** \| 

Removing and installing the FlexStack cable can shorten its useful life. Do not remove and insert the cable more often than is absolutely necessary.

 \| \|

* * *

### Removing a FlexStack Cable

**Procedure**

* * *

\| **Step 1**   | To remove a FlexStack cable, grasp the tab on the cable connector and gently pull straight out. |
\| **Step 2**   | When you remove the FlexStack cables from the connectors, replace the dust covers to protect them from dust.

\| **Caution** \| 

Removing and installing the FlexStack cable can shorten its useful life. Do not remove and insert the cable more often than is absolutely necessary.

 \| \|

* * *

## Installing the Power Cord Retainer (Optional)

The power cord retainer is optional (part number \[PWR-CLP=]). You can order it when you order your switch, or you can order it later from your Cisco representative.

**Procedure**

* * *

\| **Step 1**   | Choose the sleeve size of the power cord retainer based on the thickness of the cord. The smaller sleeve can be snapped off and used for thin cords. |
\| **Step 2**   | Slide the retainer around the AC power cord, and pass it around the loop on the switch.

Figure 12. Inserting the Retainer Through the Lanced Loop. ![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346539.jpg)

\| 

1

 \| 

AC power cord

 \| 

3

 \| 

Sleeve for thinner power cords

 \|
\| 

2

 \| 

Power cord retainer

 \| 

4

 \| 

Loop

 \|

 \|
\| **Step 3**   | Slide the retainer through the first latch.

Figure 13. Sliding the Retainer Through the Latch  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346540.jpg)

\| 

1

 \| 

AC power cord

 \| 

3

 \| 

Latch

 \|
\| 

2

 \| 

Smaller sleeve for thin power cords

 |   |   |

 \|
\| **Step 4**   | Slide the retainer through the other latches to lock it.

Figure 14. Locking the Retainer  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346541.jpg)

\| 

1

 \| 

AC power cord

 \| 

3

 \| 

Latches

 \|
\| 

2

 \| 

Sleeve for thin power cords

 |   |   |

 \|
\| **Step 5**   | (Optional) Use the small sleeve for thin power cords. Use the small sleeve to provide greater stability for thin cords. Detach the sleeve, and slide it over the power cord.

Figure 15. Sleeve Around the Power Cord  

![](https://www.cisco.com/en/US/i/200001-300000/200001-210000/208001-209000/208987.jpg)

\| 

1

 \| 

Sleeve for thin power cords

 \| 

2

 \| 

AC power cord

 \|

 \|
\| **Step 6**   | Secure the AC power cord by pressing on the retainer.

Figure 16. Securing the Power Cord in the Retainer  

![](https://www.cisco.com/en/US/i/200001-300000/200001-210000/208001-209000/208988.jpg)

 \|

* * *

## Installing SFP and SFP+ Modules

Some switch models support SFP modules, SFP+ modules, or both. The _SFP_ slots support only the SFP modules. The slots marked _SFP+_ slots support both SFP and SFP+ modules.

See the switch release notes on Cisco.com for the list of supported SFP modules. Use only Cisco SFP modules on the switch. Each Cisco module has an internal serial EEPROM that is encoded with security information. This encoding provides a way for Cisco to identify and validate that the module meets the requirements for the switch.

For information about installing, removing, cabling, and troubleshooting SFP modules, see the module documentation that shipped with your device.

-   [Installing an SFP or SFP+ Module](#ID526)
-   [Removing an SFP or SFP+ Module](#ID555)

**Related References**  

[SFP and SFP+ Module Slots](b_c2960x_hig_chapter_01.html#ID242)

### Installing an SFP or SFP+ Module

**Before You Begin**

When installing SFP or SFP+ modules, observe these guidelines:

-   Do not remove the dust plugs from the modules or the rubber caps from the fiber-optic cable until you are ready to connect the cable. The plugs and caps protect the module ports and cables from contamination and ambient light.
-   To prevent ESD damage, follow your normal board and component handling procedures when connecting cables to the switch and other devices.
    \| ![](https://www.cisco.com/en/US/i/templates/caut.gif)

    **Caution** \|   

    * * *

    Removing and installing an SFP or SFP+ module can shorten its useful life. Do not remove and insert any module more often than is absolutely necessary.

    * * *

     \|

**Procedure**

* * *

\| **Step 1**   | Attach an ESD-preventive wrist strap to your wrist and to a bare metal surface. |
\| **Step 2**   | Find the send (TX) and receive (RX) markings on the module top.

On some SFP or SFP+ modules, the send and receive (TX and RX) markings might be replaced by arrows that show the direction of the connection.

 \|
\| **Step 3**   | If the module has a bale-clasp latch, move it to the open, unlocked position. |
\| **Step 4**   | Align the module in front of the slot opening, and push until you feel the connector snap into place. |
\| **Step 5**   | If the module has a bale-clasp latch, close it. |
\| **Step 6**   | For fiber-optic SFP or SFP+ modules, remove the dust plugs and save. |
\| **Step 7**   | Connect the SFP cables.

Figure 17. Installing an SFP Module  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346534.jpg)

 \|

* * *

### Removing an SFP or SFP+ Module

**Procedure**

* * *

\| **Step 1**   | Attach an ESD-preventive wrist strap to your wrist and to a bare metal surface. |
\| **Step 2**   | Disconnect the cable from the SFP module. For reattachment, note which cable connector plug is send (TX) and which is receive (RX). |
\| **Step 3**   | Insert a dust plug into the optical ports of the SFP or SFP+ module to keep the optical interfaces clean. |
\| **Step 4**   | If the module has a bale-clasp latch, pull the bale out and down to eject the module. If the latch is obstructed and you cannot use your finger, use a small, flat-blade screwdriver or other long, narrow instrument to open the latch. |
\| **Step 5**   | Grasp the SFP or SFP+ module, and carefully remove it from the module slot. |
\| **Step 6**   | Place the module in an antistatic bag or other protective environment. |

* * *

## Connecting to SFP and SFP+ Modules

**Related References**  

[SFP and SFP+ Module Slots](b_c2960x_hig_chapter_01.html#ID242)

### Connecting to Fiber-Optic SFP and SFP+ Modules

\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Class 1 laser product. Statement 1008

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/caut.gif)

**Caution** \|   

* * *

Do not remove the rubber plugs from the SFP or SFP+ module port or the rubber caps from the fiber-optic cable until you are ready to connect the cable. The plugs and caps protect the SFP module ports and cables from contamination and ambient light. Before connecting to the SFP module, be sure that you understand the port and cabling stipulations.

* * *

 \|

**Procedure**

* * *

\| **Step 1**   | Remove the rubber plugs from the module port and fiber-optic cable, and store them for future use. |
\| **Step 2**   | Insert one end of the fiber-optic cable into the SFP or SFP+ module port. |
\| **Step 3**   | Insert the other cable end into a fiber-optic receptacle on a target device.

Figure 18. Connecting to a Fiber-Optic SFP Module Port  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346543.jpg)

\| 

1

 \| 

LC connector

 \|

 \|
\| **Step 4**   | Observe the port status LED.

The LED turns green when the switch and the target device have an established link.

The LED turns amber while the STP discovers the network topology and searches for loops. This process takes about 30 seconds, and then the port LED turns green.

If the LED is off, the target device might not be turned on, there might be a cable problem, or there might be problem with the adapter installed in the target device.

 \|

* * *

**Related References**  

[SFP Module Connectors](b_c2960x_hig_appendix_0101.html#ID35)

### Connecting to 1000BASE-T SFP

When connecting to a 1000BASE-T device, be sure to use a four twisted-pair, Category 5 or higher cable.

\| ![](https://www.cisco.com/en/US/i/templates/note.gif)

**Note** \|   

* * *

The automatic medium-dependent interface crossover (auto-MDIX) feature is enabled by default. For configuration information for this feature, see the switch software configuration guide or the switch command reference on Cisco.com.

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/caut.gif)

**Caution** \|   

* * *

To prevent ESD damage, follow your normal board and component handling procedures.

* * *

 \|

**Procedure**

* * *

\| **Step 1**   | Connect one end of the cable to the SFP module port. Insert a four twisted-pair, straight-through cable when you connect to servers, workstations, and routers. Insert a four twisted-pair, crossover cable when you connect to switches or repeaters. |
\| **Step 2**   | Connect the other end of the cable to an RJ-45 connector on the other device.

Figure 19. Connecting to a 1000BASE-T SFP Module  

![](https://www.cisco.com/en/US/i/300001-400000/340001-350000/346001-347000/346542.jpg)

\| 

1

 \| 

RJ-45 connector

 \|

 \|
\| **Step 3**   | Observe the port status LED.

-   The LED turns green when the switch and the other device have an established link.
-   The LED turns amber while the STP discovers the network topology and searches for loops. This process takes about 30 seconds, and then the port LED turns green.
-   If the LED is off, the other device might not be turned on, there might be a cable problem, or there might be a problem with the adapter in the other device.

     \|
    \| **Step 4**   | If necessary, reconfigure and restart the switch or other device. |

* * *

## 10/100/1000 PoE+ Port Connections

The ports provide PoE support for devices compliant with IEEE 802.3af and 802.3at (PoE+), and also provide Cisco prestandard PoE support for Cisco IP Phones and Cisco Aironet Access Points.

On a per-port basis, you can control whether or not a port automatically provides power when an IP phone or an access point is connected.

To access an advanced PoE planning tool, use the Cisco Power Calculator available on Cisco.com at this URL: [http:/​/​tools.cisco.com/​cpc/​launch.jsp](http://tools.cisco.com/cpc/launch.jsp)

You can use this application to calculate the power supply requirements for a specific PoE configuration. The results show output current, output power, and system heat dissipation.

\| ![](https://www.cisco.com/en/US/i/templates/warn.gif)

**Warning** \|   

* * *

Voltages that present a shock hazard may exist on Power over Ethernet (PoE) circuits if interconnections are made using uninsulated exposed metal contacts, conductors, or terminals. Avoid using such interconnection methods, unless the exposed metal parts are located within a restricted access location and users and service people who are authorized within the restricted access location are made aware of the hazard. A restricted access area can be accessed only through the use of a special tool, lock and key or other means of security. Statement 1072

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/caut.gif)

**Caution** \|   

* * *

Category 5e and Category 6 cables can store high levels of static electricity. Always ground the cables to a suitable and safe earth ground before connecting them to the switch or other devices.

* * *

 \|
\| ![](https://www.cisco.com/en/US/i/templates/caut.gif)

**Caution** \|   

* * *

Noncompliant cabling or powered devices can cause a PoE port fault. Use only standard-compliant cabling to connect Cisco prestandard IP Phones and wireless access points, IEEE 802.3af, or 802.3at (PoE+)-compliant devices. You must remove any cable or device that causes a PoE fault.

* * *

 \|

**Procedure**

* * *

\| **Step 1**   | Connect one end of the cable to the switch PoE port. |
\| **Step 2**   | Connect the other end of the cable to an RJ-45 connector on the other device. The port LED turns on when both devices have established link.

The port LED is amber while STP discovers the topology and searches for loops. This process takes about 30 seconds, and then the port LED turns green. If the LED is off, the other device might not be turned on, there might be a cable problem, or there might be a problem with the adapter in the other device.

 \|
\| **Step 3**   | Reconfigure and reboot the connected device if needed. |
\| **Step 4**   | Repeat Steps 1 through 3 to connect each device.

\| **Note** \| 

Many legacy powered devices, including older Cisco IP phones and access points that do not fully support IEEE 802.3af, might not support PoE when connected to the switches by a crossover cable.

 \| \|

* * *

## 10/100/1000 Port Connections

The switch 10/100/1000 port configuration changes to operate at the speed of the attached device. If the attached ports do not support autonegotiation, you can manually set the speed and duplex parameters. Connecting devices that do not autonegotiate or that have the speed and duplex parameters manually set can reduce performance or result in no linkage.

To maximize performance, choose one of these methods for configuring the Ethernet ports:

-   Let the ports autonegotiate both speed and duplex.

-   Set the interface speed and duplex parameters on both ends of the connection.

-   [Auto-MDIX Connections](#ID669)

### Auto-MDIX Connections

The autonegotiation and the auto-MDIX features are enabled by default on the switch.

With autonegotiation, the switch port configurations change to operate at the speed of the attached device. If the attached device does not support autonegotiation, you can manually set the switch interface speed and duplex parameters.

With auto-MDIX, the switch detects the required cable type for copper Ethernet connections and configures the interface accordingly.

If auto-MDIX is disabled, use the guidelines in this table to select the correct cable.  

Table 1 Recommended Ethernet Cables (When Auto-MDIX is Disabled)
\| 

Device

 \| 

Crossover Cable

 \| 

Straight-Through Cable

|  |
|  |

\| 

Switch to switch

 \| 

Yes

 \| 

No

 \|
\| 

Switch to hub

 \| 

Yes

 \| 

No

 \|
\| 

Switch to computer or server

 \| 

No

 \| 

Yes

 \|
\| 

Switch to router

 \| 

No

 \| 

Yes

 \|
\| 

Switch to IP phone

 \| 

No

 \| 

Yes

 \|

1 100BASE-TX and 1000BASE-T traffic requires twisted four-pair, Category 5, Category 5e, or Category 6 cable. 10BASE-T traffic can use Category 3 or Category 4 cable.

## Where to Go Next

If the default configuration is satisfactory, the switch does not need further configuration. You can use any of these management options to change the default configuration:

-   Start the Network Assistant application, which is described in the getting started guide. Through this GUI, you can configure and monitor a switch cluster or an individual switch.
-   Use the CLI to configure the switch as a member of a cluster or as an individual switch from the console.
-   Use the Cisco Prime Infrastructure application.
-   [https://www.cisco.com/en/US/docs/switches/lan/catalyst2960x/hardware/installation/guide/b_c2960x_hig_chapter_010.html#ID254](https://www.cisco.com/en/US/docs/switches/lan/catalyst2960x/hardware/installation/guide/b_c2960x_hig_chapter_010.html#ID254)
