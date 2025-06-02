# Space Mission Scenarios

## Introduction

Below are mission scenarios for implementation within the laboratory using GMAT and VTS Timeloop software. Part A focuses on exploring orbital parameters in GMAT and is intended for individual work. Part B concentrates on visualizing specific types of missions and sensor operations in VTS Timeloop and is designed for group work.

-----

## Part A: Scenarios for GMAT (Individual Work)

**Objective:** To understand the influence of individual Keplerian elements on the characteristics of a satellite's orbit.

**For each of the following 10 scenarios:**

1.  Configure a mission in GMAT, modifying the indicated Keplerian parameters. Use an initial date of `01 Jan 2025 12:00:00.000 UTCG` and a spacecraft mass of 1000 kg (unless otherwise specified). Use an Earth gravity model (e.g., JGM-2 4x4) and, if indicated, atmospheric drag (e.g., JacchiaRoberts).
2.  Run the simulation for at least 3 orbits or for 24 hours (unless the scenario suggests otherwise).
3.  Observe and record in your report:
      * The shape of the orbit in the 3D view (`OrbitView`).
      * The ground track (`GroundTrackPlot`).
      * Key orbital parameters (periapsis/apoapsis altitude, period).
4.  Answer the questions related to each scenario.

-----

**Scenario A1: Baseline Low Earth Orbit (LEO) - Circular**

  * **Keplerian Parameters:**
      * Semi-Major Axis (SMA): `6971 km` (altitude approx. 600 km)
      * Eccentricity (ECC): `0.001`
      * Inclination (INC): `51.6 deg`
      * Right Ascension of the Ascending Node (RAAN): `0 deg`
      * Argument of Periapsis (AOP): `0 deg`
      * True Anomaly (TA): `0 deg`
  * **Questions:** What is the approximate orbital period? What latitudes are regularly overflown by the satellite?

**Scenario A2: Effect of Semi-Major Axis (SMA)**

  * **Base Parameters:** INC = `51.6 deg`, RAAN = `0 deg`, AOP = `0 deg`, TA = `0 deg`.
  * Maintain ECC, INC, RAAN, AOP, TA as in A1. Only change SMA:
      * a) SMA = `6771 km` (LEO, alt. \~400 km)
      * b) SMA = `26560 km` (MEO, GPS-like orbit)
      * c) SMA = `42164 km` (GEO, geostationary orbit)
  * **Questions:** How does changing SMA affect the orbital period (`MySat.Earth.OrbitPeriod`), satellite velocity (`MySat.Earth.VMAG`), and the area of Earth visible from the satellite (intuitively)? Compare the ground tracks.

**Scenario A3: Effect of Eccentricity (ECC)**

  * **Base Parameters:** SMA = `42164 km`, INC = `0 deg`, RAAN = `0 deg`, AOP = `0 deg`, TA = `0 deg`.
  * Vary ECC:
      * a) ECC = `0.005` (nearly circular)
      * b) ECC = `0.1`
      * c) ECC = `0.4` (elliptical)
  * **Questions:** How does ECC change the shape of the orbit? Where are the periapsis and apoapsis located? How does the satellite's velocity change at these points?

**Scenario A4: Effect of Inclination (INC)**

  * **Base Parameters:** SMA = `6971 km`, ECC = `0.001`, RAAN = `0 deg`, AOP = `0 deg`, TA = `0 deg`.
  * Vary INC:
      * a) INC = `0 deg` (equatorial)
      * b) INC = `90 deg` (polar)
      * c) INC = `28.5 deg` (e.g., for a Florida launch site)
  * **Questions:** How does inclination affect the maximum and minimum latitude range of the ground track? What are the advantages of a polar orbit for global coverage?

**Scenario A5: Effect of Right Ascension of the Ascending Node (RAAN)**

  * **Base Parameters:** SMA = `6971 km`, ECC = `0.001`, INC = `51.6 deg`, AOP = `0 deg`, TA = `0 deg`.
  * Vary RAAN:
      * a) RAAN = `0 deg`
      * b) RAAN = `90 deg`
      * c) RAAN = `180 deg`
  * **Questions:** How does RAAN affect the orientation of the orbital plane with respect to the vernal equinox (and consequently, with respect to the Sun at a given time)? How does the location of the ascending node on the equator change?

**Scenario A6: Effect of Argument of Periapsis (AOP)**

  * **Base Parameters:** SMA = `6971 km`, ECC = `0.001`, INC = `51.6 deg`, RAAN = `0 deg`, TA = `0 deg`.
  * Vary AOP:
      * a) AOP = `0 deg`
      * b) AOP = `90 deg`
      * c) AOP = `270 deg`
  * **Questions:** How does AOP affect the position of the periapsis within its orbital plane? How does this influence the geographic latitude of the periapsis (for a given inclination and RAAN)?

**Scenario A7: Geostationary Orbit (GEO)**

  * **Task:** Adjust SMA, ECC, INC parameters to achieve an orbit as close as possible to geostationary (period \~23h 56m 4s, ECC = 0, INC = 0).
      * SMA = `42164 km`
      * ECC = `0`
      * INC = `0 deg`
  * **Questions:** Why are GEO orbits crucial for telecommunications? What does the ground track of an ideal GEO orbit look like? What happens to the track with a non-zero inclination (inclined geosynchronous orbit)?

**Scenario A8: Molniya Orbit (HEO)**

  * **Parameters:**
      * SMA = `26560 km` (period \~12h)
      * ECC = `0.74`
      * INC = `63.4 deg`
      * AOP = `270 deg` (apoapsis over the northern hemisphere)
      * RAAN = `0 deg`, TA = `0 deg`
  * **Questions:** What is the characteristic feature of this orbit regarding the satellite's dwell time over specific Earth regions? Why is the `63.4 deg` inclination "critical" for such orbits?

**Scenario A9: Sun-Synchronous Orbit (SSO)**

  * **Parameters:** SMA = `7071 km` (altitude approx. 700 km), ECC = `0.001`.
  * **Task:** Select an appropriate inclination (usually `97-99 deg`, retrograde orbit). If possible in GMAT, demonstrate RAAN precession (may require longer propagation time (`MySat.ElapsedDays  
   `) and enabled force model).
  * **Questions:** What are the advantages of an SSO orbit for Earth observation missions (e.g., consistent lighting conditions)? What does "sun-synchronous" mean?

**Scenario A10: Effect of Atmospheric Drag on LEO**

  * **Base Parameters:** SMA = `6678 km` (altitude \~300 km - low LEO), ECC = `0.001`, INC = `51.6 deg`.
  * Configure the spacecraft with `DragArea = 10 m^2`, `DragCoefficient = 2.2`, `DryMass = 1000 kg`.
  * Enable an atmosphere model (e.g., `JacchiaRoberts`) in the propagator.
  * Propagate the mission for several days (e.g., 7 days).
  * **Questions:** How does atmospheric drag affect the orbit altitude (SMA and periapsis/apoapsis altitude) over time? What does this mean for the lifetime of a satellite in low orbit?

-----

## Part B: Scenarios for VTS Timeloop (Group Work)

**Objective:** To visualize specific types of satellite missions and their sensor operations, understanding the relationship between the orbit and mission capabilities.

**For each of the following 5 scenarios (each group chooses one):**

1.  **Propose and justify** appropriate orbital parameters (Keplerian elements) for the given mission. You can base this on knowledge from Part A.
2.  **Configure the satellite and its orbit in VTS Timeloop.** (Data can be entered manually via [TLE2CCSDS plugin](https://timeloop.fr/static/doc/manual/pages/Data_generators_in_VTS/index.html#orbit-propagation-with-the-tle2ccsds-generator) or, if feasible, import an ephemeris file from GMAT in CCSDS OEM format if the group decides to first simulate the orbit in GMAT).
3.  **Define and configure sensor(s)** appropriate for the mission, specifying their type, shape, range/beam size, and orientation.
4.  **Conduct a visualization of the mission's operation**, observing aspects like ground coverage, visibility, accessibility.
5.  **Prepare a short presentation** (e.g., 5-10 minutes) for the other groups, including: mission description, justification for orbit choice, sensor parameters, key screenshots/animations from VTS, and conclusions.

-----

**Scenario B1: Global Weather Observation Satellite**

  * **Mission:** Continuous monitoring of global weather systems, clouds, sea surface temperatures.
  * **Suggested Orbit:** LEO, polar or near-polar (e.g., SSO for consistent illumination).
  * **Sensor(s):**
      * Primary: Wide-swath radiometer/imager (e.g., simulating a scan swath of 2000-3000 km).
      * Optional: Atmospheric sounder (e.g., narrow nadir-pointing beam).
  * **Visualization Tasks:** Show daily Earth coverage. How often is a given area revisited? What are the limitations?

**Scenario B2: Regional Telecommunications Satellite**

  * **Mission:** Provide telecommunication services (TV, internet, telephony) over a specific region (e.g., Europe, Middle East).
  * **Suggested Orbit:** Geostationary (GEO) or Inclined Geosynchronous Orbit (IGSO) for certain regions.
  * **Sensor(s):** Several transmit/receive antennas (transponders) with different beam shapes (e.g., a wide continental beam, several narrower "spot beams" covering key areas).
  * **Visualization Tasks:** Show the beam footprints on the Earth's surface. What are the pros and cons of GEO for this application? What if the region is at a high latitude?

**Scenario B3: High-Resolution Earth Observation Satellite**

  * **Mission:** Deliver detailed images of selected targets on Earth on demand (e.g., infrastructure monitoring, disaster response, precision agriculture).
  * **Suggested Orbit:** LEO, Sun-Synchronous Orbit (SSO) for optimal and repeatable lighting conditions.
  * **Sensor(s):** Narrow-angle, high-resolution optical telescope/camera (e.g., simulating a scan swath of 10-20 km). Ability to slew the sensor HSPB sideways ("off-nadir" observations).
  * **Visualization Tasks:** Show the capability to image a specific target. How often can the same target be imaged (revisit time)? How does the sensor slewing capability affect revisit frequency and the accessible imaging area?

**Scenario B4: Navigation Satellite Constellation**

  * **Mission:** Provide global or regional positioning, navigation, and timing (PNT).
  * **Suggested Orbit:** MEO (e.g., altitude \~20,000-23,000 km, inclination \~55-56°, multiple orbital planes).
  * **Sensor(s):** Antenna emitting a navigation signal (wide beam covering a large portion of the Earth visible from the satellite).
  * **Visualization Tasks:** (Simplification: the group can focus on one satellite from the constellation or a few in one plane). Show the signal coverage area from one satellite. Discuss why a constellation of many satellites is needed for continuous global coverage and accuracy.

**Scenario B5: Research Satellite for Space Debris Monitoring**

  * **Mission:** Detect, track, and catalog space debris objects in Earth orbits.
  * **Suggested Orbit:** LEO, potentially with the ability to change altitude or inclination within a certain range (or the group chooses a specific orbit to monitor, e.g., popular LEO altitudes).
  * **Sensor(s):**
      * Optical: Telescope with a specific field of view for object detection.
      * Radar (optional, conceptual): Radar with a defined range and "field of regard."
  * **Visualization Tasks:** Show how the satellite's sensor can scan space for objects. What are the challenges in detecting small objects? Which orbits are the most "crowded"?

-----

Remember to approach the tasks creatively, especially in Part B. The goal is not only to correctly configure the software but also to understand the practical aspects of space mission design. Good luck\!
