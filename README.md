# Autonomous Navigation System

**[日本語のREADMEはこちら](READM-jp.md)**

This project showcases a field-proven autonomous navigation system for marine vessels. Developed over a decade of real-world testing, this system is designed to guide a boat to its destination while intelligently avoiding obstacles, even in dense maritime traffic.

The video clips below are not simulations or cherry-picked highlights. They are excerpts from longer, unedited voyages that demonstrate the system's reliability and precision in real-time.

## Key Features

*   **Fully Autonomous Navigation:** The system handles obstacle detection, path planning, and vessel control from start to finish.
*   **Real-World Reliability:** Proven through extensive, unedited long-distance voyages in busy waters.
*   **Cost-Effective Design:** Built with readily available, off-the-shelf components, making it adaptable to a wide range of vessels.
*   **Advanced Sensor Fusion:** Integrates data from radar, AIS, and GPS for robust situational awareness.

## Demonstrations

To witness the system's capabilities, please watch the four short video clips below. For those who may doubt the authenticity of these demonstrations, please note that each clip is an excerpt from a much longer, unedited voyage. The full, unedited videos are available on the same YouTube channel, providing verifiable proof of the system's real-world performance.

[![Collision Avoidance: Distant Vessel in Open Water](https://img.youtube.com/vi/K0jlUjXqyp0/0.jpg)](https://youtu.be/K0jlUjXqyp0)

**Title:** Autonomous Collision Avoidance: Distant Vessel in Open Water

**Description:** This video demonstrates typical collision avoidance at a distance. In open water, the autonomous navigation system plans a slightly adjusted course early on, so dramatic, last-minute maneuvers are rarely necessary. This makes capturing obvious avoidance actions on camera less common.

[![AIS-Assisted Collision Avoidance: Tugboat at Port Entrance](https://img.youtube.com/vi/Kf-5UJ2-694/0.jpg)](https://youtu.be/Kf-5UJ2-694)

**Title:** AIS-Assisted Collision Avoidance: Tugboat at Port Entrance

**Description:** This video shows the system navigating past a tugboat at a port entrance amidst a strong wake. Because the tugboat has Class A AIS, its position is tracked with high precision by fusing AIS and radar data. While navigating such a narrow channel would typically require manual control, the enhanced accuracy from AIS fusion allows the autonomous system to confidently execute a safe, close pass.

[![Navigating Heavy Traffic: Close Quarters with a Fishing Boat at Port Entrance](https://img.youtube.com/vi/Uqsw_XnjuP8/0.jpg)](https://youtu.be/Uqsw_XnjuP8)

**Title:** Navigating Heavy Traffic: Close Quarters with a Fishing Boat at Port Entrance

**Description:** At the busy port entrance, with numerous boats nearby, the system demonstrates intelligent speed control. It initially waits for a fishing boat approaching from behind to pass, as it exceeds the pre-configured speed limit for the area. Once the boat has passed, the system predicts its trajectory, smoothly accelerates, and precisely follows in its wake.

[![Complex Collision Avoidance: Barge Under Tow](https://img.youtube.com/vi/0gb-SchXd7Q/0.jpg)](https://youtu.be/0gb-SchXd7Q)

**Title:** Complex Collision Avoidance: Barge Under Tow

**Description:** During a fishing trip, the system encountered a square-shaped barge under tow. Due to the barge's unusual shape, the radar-based position and heading estimation fluctuated with distance, causing the system to hesitate momentarily as it recalculated the safest path. This clip highlights the challenge of tracking irregularly shaped objects.

## System Overview

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

## Project History
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

## Source Code

The source code for this project is not currently public. An older version, last updated around 2020, can be found on my GitHub profile, but it does not reflect the system's current capabilities. I am in the process of preparing the codebase for a future release.

## Links

*   **YouTube Channel:** [https://www.youtube.com/@yhmtmt/featured](https://www.youtube.com/@yhmtmt/featured)
*   **GitHub:** [https://github.com/yhmtmt/](https://github.com/yhmtmt/)
*   **Blog (2010-2018):** [http://blog.livedoor.jp/yhmtmt/](http://blog.livedoor.jp/yhmtmt/)

## About Me

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

## Contact Information

Yohei Matsumoto  
yhmtmt@gmail.com