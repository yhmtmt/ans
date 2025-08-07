# Autonomous Navigation System

This autonomous navigation system is designed to guide marine vessels to their destination without human intervention. While various levels of autonomy exist, this system aims for the highest degree: it autonomously 1) detects and tracks obstacles, 2) plans the optimal path to the destination, and 3) controls the vessel along that path. Although still under development, its current precision allows for hands-free navigation even in busy marine traffic.

To witness its capabilities, please watch these four short video clips of my boat autonomously avoiding other vessels, clearly demonstrating that I am not controlling it manually:

<Here we have four YouTube links.>

You might wonder if someone onboard is secretly controlling the boat, or if these clips are merely highlights from numerous attempts. The answer is no—this is real. What sets this system apart is that these clips are taken from longer, unedited videos of successful navigations, some of which clearly show me alone on the boat. I have uploaded the full, unedited versions to the same YouTube channel; feel free to verify this. Watching any of them will confirm that achieving such continuous success through repeated trials would be unrealistic. These video files were casually recorded during my weekend fishing trips.

Many startups and research groups are actively developing autonomous marine navigation solutions. However, I have yet to see anyone demonstrate this level of capability with verifiable evidence. Promotional videos often rely on computer graphics, or they omit crucial context—for instance, obstacle information might be externally provided, the boat might simply be following a pre-programmed route, or only the best takes from many trials are shown. I am confident that my system represents a fundamentally different level of achievement: genuine, real-world autonomous navigation.

# History
 In 2015, I began developing this autonomous navigation system on my personal boat, a YAMAHA YF-24 named AWS-1. For the past decade, this project has been a self-funded passion and a solitary hobby pursued alongside my fishing trips. (As such, this is entirely my private work, and I retain all rights, a fact recently verified by my employer.)

Today, the system has achieved a level of maturity that makes it practical for real-world application. As evidence, I have uploaded numerous long, unedited videos demonstrating the boat successfully navigating the busy traffic of my local port without human intervention, even in relatively rough seas.

I am now planning to scale up this project. If you are interested or have a proposal, please feel free to contact me.

*   **July 2015:** Project started at Urayasu Marina.
*   **February 2016:** Implemented electrical control of the rudder and engine throttle (using Jetson TK1 and Zedboard).
*   **May 2016:** Achieved waypoint navigation with engine control.
*   **April 2018:** Updated the autopilot and added remote control via Android Watch or phone (using Jetson TX1 and Zybo Z7).
*   **November 2021:** Implemented radar-based collision avoidance (using Jetson AGX Xavier and Zybo Z7).
*   **November 2022:** Developed a unified collision avoidance algorithm with a path planner.
*   **August 2023:** Moved the project to Kisarazu Marina (an area with extremely dense boat traffic).
*   **2023-2025:** Refined algorithms and uploaded many unedited YouTube videos of successful, unassisted runs through the Kisarazu Port.
*   **March 2025:** Migrated the system to a more cost-effective setup using a Raspberry Pi 5 and Renesas RA8E1.
*   **July 2025:** The entire development was officially verified as my private work by the Tokyo University of Marine Science and Technology (my current employer).

# System Overview

The system is built upon a YAMAHA YF-24, utilizing standard marine equipment. A Marol actuator and helm pump facilitate control over the engine and rudder. For comprehensive situational awareness, the system integrates data from a GARMIN HD 18x Radar and a cost-effective AIS receiver. These components are interfaced with a Raspberry Pi 5 (8GB) via a Renesas RA8E1 microcontroller, which then executes the autonomous navigation software.

```mermaid
graph TD;
    classDef hardware fill:#cccccc,stroke:#333,stroke-width:2px,color:#000;
    classDef software fill:#a0c0e0,stroke:#333,stroke-width:2px,color:#000;

    subgraph "Sensors"
        Radar[Radar]
        AIS[AIS Receiver]
        GPS[GPS Compass]
        EngineStatus[Engine Status]
    end

    subgraph "Navigation System"
        Microcontroller[Renesas RA8E1]
            ANS[Raspberry Pi 5]
    end

    subgraph "Actuators"
        HelmPump[Helm Pump]
        Actuator[Actuator]
        Rudder[Rudder]
        Engine[Engine]
    end

    Radar -- Ethernet --> ANS;
    AIS -- NMEA0183 --> Microcontroller;
    GPS -- NMEA0183 --> Microcontroller;
    EngineStatus -- NMEA2000 --> Microcontroller;
    Microcontroller -- Sensor Data --> ANS;
    ANS -- Control Commands --> Microcontroller;
    Microcontroller -- Analog Signal --> HelmPump;
    Microcontroller -- Analog Signal --> Actuator;
    HelmPump --> Rudder;
    Actuator --> Engine;

    class ANS,Radar,AIS,GPS,EngineStatus,Microcontroller,HelmPump,Actuator,Rudder,Engine hardware;
 ```

This system is remarkably cost-effective. The only additional hardware required for a typical boat are the computers, which amount to only a few hundred dollars.

The system autonomously detects surrounding obstacles, estimates their state (position, course, speed), plans a safe path to the destination, and controls the boat to follow that plan. Key technologies employed include sensor fusion, advanced radar signal processing, and intelligent path planning algorithms.

Currently, in open water, the system requires human intervention only once every few days of operation. While it is not yet capable of full, unattended autonomy, I believe this can be achieved by enhancing the precision of each component. The AI-based modules are still less developed compared to those in the automotive industry, primarily due to a lack of large-scale data collection and processing facilities. Further improvements are anticipated with more significant investment and scaled-up development.

The current system may not appear highly advanced, as it is installed on a standard 24ft boat and features a somewhat loud, exposed helm pump. However, this setup effectively demonstrates that the system can be retrofitted onto older, reliable boats that many people depend on. While it has only been tested on my YF-24 with my specific equipment, I believe the system could be adapted to other vessels, provided the necessary interfaces to the boat's existing equipment can be established. 


# Links

*   **YouTube Channel:** [https://www.youtube.com/@yhmtmt/featured](https://www.youtube.com/@yhmtmt/featured)
*   **GitHub:** [https://github.com/yhmtmt/](https://github.com/yhmtmt/)
*   **Blog (2010-2018):** [http://blog.livedoor.jp/yhmtmt/](http://blog.livedoor.jp/yhmtmt/)

# About Me

<table>
  <tr>
    <td width="400">
      <img src="https://img.youtube.com/vi/enGqSmd5rLU/maxresdefault.jpg" alt="Autonomous Navigation System" width="300"/>
    </td>
    <td>
      <strong>Yohei Matsumoto</strong><br><br>
      I received my PhD from Okayama University in 2005, specializing in FPGA interconnection architecture and CAD algorithms. Subsequently, I joined the National Institute of Advanced Industrial Science and Technology (AIST), where I continued my research in the same field. In 2009, I transitioned to the Tokyo University of Marine Science and Technology. My private project, the autonomous navigation system, began in 2015 as a weekend hobby.
    </td>
  </tr>
</table>

# Contact Information

Yohei Matsumoto  
yhmtmt@gmail.com
