# GMAT and VTS Timeloop Software Manual

## Introduction

This manual aims to guide you through the basic functionalities of NASA's General Mission Analysis Tool (GMAT) and VTS Timeloop, which will be used during the laboratory. It is recommended to thoroughly familiarize yourself with the steps below before proceeding with the mission scenarios.

-----

## Part 1: General Mission Analysis Tool (GMAT)

### 1.1. Installation and First Launch

  * **Installation:** Follow the instructions available on the official GMAT website (https://software.nasa.gov/software/GSC-17177-1). Ensure you download the version appropriate for your operating system.
  * **First Launch:** After installation, launch GMAT. Familiarize yourself with the main application window.

### 1.2. Overview of the GMAT User Interface

The main components of the GMAT interface are:

  * **Resources Tree:** On the left side, contains all mission elements (spacecraft, propagators, coordinate systems, plots, reports, etc.).
  * **Script Editor Panel:** On the right side, displays the mission script in GMAT's scripting language. You can edit the mission textually here.
  * **Message Window Panel:** At the bottom, displays logs, errors, and warnings.
  * **Toolbar:** At the top, contains buttons for running simulations, saving, opening, etc.

### 1.3. Basic Steps for Creating a Mission in GMAT

The following outlines a typical workflow for creating a new mission.

**Step 1: Creating a `Spacecraft` Object**

1.  In the Resources Tree, right-click on the `Spacecraft` folder and select `Add Spacecraft`.
2.  Name your spacecraft (e.g., `MySat`).
3.  In the opened spacecraft configuration window, you can define its parameters:
      * `Epoch`: Initial date and time of the mission (e.g., `01 Jan 2025 12:00:00.000 UTCG`).
      * `CoordinateSystem`: The coordinate system in which you define the initial state (e.g., `EarthMJ2000Eq` - Earth Mean J2000 Equatorial).
      * `DisplayStateType`: Type of orbital elements for display and editing (e.g., `Keplerian`).
      * Enter values for the 6 Keplerian elements (SMA, ECC, INC, RAAN, AOP, TA/MA - [see scenarios](SCENARIOS.md)).
      * You can also define mass (`DryMass`), ballistic coefficients (`DragCoefficient`, `DragArea`), solar radiation pressure coefficients, etc., if required by the force model.
4.  Click `Apply` and `OK`.

**Step 2: Defining Coordinate Systems (if different from default)**

  * GMAT uses standard systems by default (e.g., `EarthMJ2000Eq`, `EarthFixed`). Usually, there's no need to create new ones for basic LEO missions.

**Step 3: Configuring the Propagator (e.g., `Prop_Default`)**

1.  In the Resources Tree, find the `Propagators` folder, and within it, the default propagator (e.g., `DefaultProp`). Double-click it.
2.  In the propagator configuration window:
      * `ForceModel`: Select the force model. Click `Edit` next to `ForceModel`.
          * `PrimaryBody`: Usually `Earth`.
          * `GravityField`: Earth's gravity field model (e.g., `JGM-2` or `JGM-3` of a specific degree and order, e.g., `20x20`).
          * `Atmosphere`: Atmosphere model (e.g., `JacchiaRoberts` or `MSISE90`), if you want to include drag.
          * `SolarRadiationPressure`: If you want to include solar radiation pressure.
          * Add other forces as needed (e.g., Moon's gravity, Sun's gravity).
      * `Integrator`: Type of numerical integrator (e.g., `RungeKutta89`).
      * `InitialStepSize`: Initial time step for integration (e.g., `60` seconds).
3.  Click `Apply` and `OK`.

**Step 4: Defining Mission Commands (Mission Sequence)**

1.  In the Resources Tree, find `Mission Sequence` (or `Script Event` in older versions). Double-click to open the script editor.
2.  A basic mission sequence for simple propagation might look like this:
    ```gmat
    Propagate DefaultProp(MySat) {MySat.ElapsedSecs = 86400.0}; // Propagate for 86400 seconds (1 day)
    ```
    Or propagate until a certain number of orbits are completed:
    ```gmat
    Propagate DefaultProp(MySat) {MySat.Earth.PeriapsisCount = 3}; // Propagate for 3 periapsis passages
    ```

**Step 5: Generating Output Data**

  * **`ReportFile` (Text Report File):**

    1.  In the Resources Tree, right-click on `ReportFiles` and select `Add ReportFile`.
    2.  Give it a name (e.g., `OrbitDataReport`).
    3.  In the configuration window:
          * `Filename`: Name of the output file (e.g., `orbit_data.txt`).
          * `Parameters`: Add parameters you want to save (e.g., `MySat.UTCGEpoch`, `MySat.EarthMJ2000Eq.X`, `MySat.EarthMJ2000Eq.Y`, `MySat.EarthMJ2000Eq.Z`, `MySat.Earth.Altitude`, `MySat.Earth.Latitude`, `MySat.Earth.Longitude`).
    4.  Add the command `Report OrbitDataReport MySat` to the mission sequence (before or inside the `Propagate` loop if you want data during propagation).

  * **`OrbitView` (3D Orbit Visualization):**

    1.  In the Resources Tree, right-click on `OrbitView` (or `OpenGLPlot` in older versions) and select `Add OrbitView`.
    2.  Give it a name (e.g., `MyOrbitView`).
    3.  In the configuration:
          * Add `MySat` to the list of objects to display.
          * Configure other options (view coordinate system, orbit visibility, Earth visibility, etc.).
    4.  No command needs to be added to the script; plots are updated automatically.

  * **`GroundTrackPlot` (Ground Track Plot):**

    1.  Similar to `OrbitView`, add an `XYPlot` (or `GroundTrackPlot`).
    2.  Configure axes: X as `MySat.Earth.Longitude`, Y as `MySat.Earth.Latitude`.

  * **Exporting Ephemeris Data (e.g., CCSDS OEM):**

      * GMAT can generate ephemeris files in the CCSDS OEM (Orbit Ephemeris Message) standard.
      * In the Resources Tree, right-click on `EphemerisFiles` and select `Add EphemerisFile`.
      * Configure the file type as `CCSDS-OEM`.
      * Specify the object (`Spacecraft`), coordinate system, epoch format, and output time step.
      * Add the command `Write MySatEphem MySat` to the mission sequence (usually after propagation is complete).
      * *Note: Availability and configuration of the CCSDS exporter may vary depending on the GMAT version.*

### 1.4. Running the Simulation and Analyzing Results

1.  **Save the mission:** Click the floppy disk icon or `File -> Save`.
2.  **Run the mission:** Click the green "Run" button (arrow).
3.  Observe messages in the `Message Window` panel.
4.  After the simulation finishes, open the generated report files and analyze the plots (`OrbitView`, `GroundTrackPlot`).

### 1.5. Tips and Common Problems

  * **Script errors:** Carefully check the syntax in the script editor. The message panel often indicates the line with the error.
  * **Unstable propagation:** If the simulation "diverges" or ends with an error, try reducing the integrator's time step (`InitialStepSize`) or simplifying the force model.
  * **Units:** Ensure you are using consistent units (GMAT defaults to km, seconds, degrees).
  * **GMAT Documentation:** Use the extensive GMAT documentation available online ([suspicious link removed]) and the built-in help.

-----

## Part 2: VTS Timeloop

### 2.1. Installation and First Launch

  * **Installation:** Follow the instructions provided by the VTS Timeloop developers or the lab administrator.
  * **First Launch:** Launch VTS Timeloop. Familiarize yourself with the interface.

### 2.2. Overview of the VTS Timeloop User Interface

The VTS Timeloop interface typically consists of:

  * **Main 3D Visualization Window:** Shows Earth, satellites, orbits, etc.
  * **Scene/Object Panel:** A list of all objects in the scene (satellites, sensors, ground stations).
  * **Time Control Panel:** Allows playing, pausing, rewinding the simulation in time, and changing speed.
  * **Menu and Toolbar:** Access to import functions, configuration, object creation.

*(Note: The exact appearance of the interface may vary depending on the VTS Timeloop version. The following steps are general.)*

### 2.3. Basic Steps for Creating a Visualization in VTS Timeloop

**Step 1: Creating a New Scene/Project**

  * Typically, a default scene is created upon program launch, or you can create a new one via the menu (`File -> New Scene` or similar).

**Step 2: Importing Mission Data or Defining a Satellite**

  * **Option A: Import from File (e.g., CCSDS OEM)**

    1.  Find the import option in the menu (e.g., `File -> Import -> Orbit Data` or `Add Object -> Satellite from File`).
    2.  Select the ephemeris file generated by GMAT (or another, e.g., in `.oem`, `.tle` format).
    3.  VTS may require configuring data mapping or import parameters. Follow the on-screen instructions.

  * **Option B: Manually Defining a Satellite and Orbit**

    1.  Add a new satellite object (e.g., `Add Object -> Satellite` or similar).
    2.  Give it a name.
    3.  In the satellite's properties panel, find the orbit section.
    4.  Enter the orbit parameters (e.g., Keplerian elements: SMA, ECC, INC, RAAN, AOP, TA/MA, and epoch).
    5.  Ensure the correct central body (Earth) and coordinate system are selected.

**Step 3: Defining and Configuring Sensors**

1.  Select the satellite to which you want to add a sensor.
2.  In the satellite's properties panel or via the context menu, find the option to add a sensor (e.g., `Add Sensor`, `Attach Payload`).
3.  Choose the sensor type (e.g., conical, pyramidal, rectangular).
4.  Configure sensor parameters:
      * **Shape and size:** E.g., cone angle for a conical sensor (`Field of View Angle`), dimensions for a rectangular sensor.
      * **Range:** Minimum and maximum operating range (if applicable).
      * **Orientation:** How the sensor is pointed relative to the satellite's axes (e.g., Nadir - towards Earth, Limb - tangential to the atmosphere, or specific angles). VTS may offer auto-pointing options (e.g., `Target Earth Center`).
      * **Visualization:** Color, transparency of the sensor beam.

**Step 4: Navigating in the 3D View and Time Control**

  * **Navigation:** Use the mouse (left button to rotate, right to pan, wheel to zoom) or keyboard shortcuts to move around the 3D scene.
  * **Time Control:** Use the time control panel to:
      * Set the initial date and time of the simulation.
      * Play (`Play`), pause (`Pause`) the simulation.
      * Change the playback speed (`Time Scale`).
      * Rewind and fast-forward time.

**Step 5: Visualizing Trajectory, Ground Track, Sensor Coverage**

  * Observe the satellite's movement along the orbit.
  * Enable display of the trajectory (full or partial).
  * Enable display of the ground track.
  * Observe how the sensor beam moves across the Earth's surface or in space. Analyze coverage areas.
  * VTS may offer tools for access analysis (e.g., visibility time between a satellite and a ground station) or calculating coverage statistics.

### 2.4. Exporting Views/Animations

  * **Screenshots:** Use VTS's built-in function (if available) or the operating system's standard function to take screenshots of interesting views.
  * **Animations:** If VTS Timeloop supports animation export (e.g., to video format), refer to the relevant options in the menu (`File -> Export Animation` or similar).

### 2.5. Tips and Common Problems

  * **Performance:** Advanced visualizations with many objects and detailed models can be resource-intensive. If you encounter problems, try simplifying the scene (e.g., reduce the number of displayed orbits, hide unnecessary objects).
  * **Coordinate Systems:** Ensure all data (orbits, orientation) are defined and interpreted in consistent coordinate systems.
  * **VTS Timeloop Documentation:** Use the official documentation, tutorials, or help provided by the program developers.
  * **File Formats:** Check which file formats are supported by your VTS Timeloop version for importing orbital data and 3D models.

-----

Remember, practice makes perfect. The more you experiment with the options in both programs, the better you will understand their capabilities. Good luck\!