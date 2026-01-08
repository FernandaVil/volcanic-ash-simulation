# Volcanic Ash Dispersion Simulation: Stochastic Modeling Engine

**Can we predict volcanic risk using physics and Python?**

This project develops a physics-based computational engine to simulate the atmospheric transport of volcanic ash using **Langevin Stochastic Differential Equations (SDEs)**. Designed as a lightweight alternative to complex meteorological models, it provides rapid risk assessment and historical reconstruction capabilities.


> 🇪🇸 [Versión en Español](./README.es.md)



![Simulation Demo](./assets/final_map.gif)
*Lagrangian particle simulation visualizing the first 5 hours of the 2015 Calbuco eruption. Red particles represent the active plume front driven by wind vectors and atmospheric turbulence*


---

## Project Workflow & Results

The engine is designed to operate in two distinct analytical modes, serving different stages of disaster management:

### Mode 1: Probabilistic Risk Assessment (Prevention)
*Focus: Statistical Analysis (Notebook 2)*

Before applying the model to a specific historical event, the engine uses **Monte Carlo simulations** with synthetic wind data to generate a **Risk Heatmap**. By calculating the particle density per grid unit, we identify theoretical accumulation zones and high-risk areas.

<div align="center">
  <img src="./assets/mapa_riesgo.png" width="70%">
  <p><i>Output: Probabilistic Heatmap (The "Red Zone" indicates high ash fall probability under uncertainty).</i></p>
</div>

* *Note: This statistical output allows authorities to make data-driven decisions under uncertainty.*
### Mode 2: Historical Validation (Calbuco 2015)
*Focus: Physics & Dynamics (Notebook 3)*

To verify the engine's physical accuracy, I calibrated the model to replicate the **2015 Calbuco Volcano eruption**. We contrasted the simulation against real satellite imagery to validate dispersion patterns.

<table>
  <tr>
    <td align="center"><b>Stochastic Simulation (Python)</b></td>
    <td align="center"><b>Reality (NASA MODIS Satellite)</b></td>
  </tr>
  <tr>
    <td align="center"><img src="./assets/pantallazo_con_zoom.png" width="400"></td>
    <td align="center"><img src="./assets/satelital_nasa.jpg" width="400"></td>
  </tr>
  <tr>
    <td align="center"><i>Prediction: NE dispersion & fan-shaped spread</i></td>
    <td align="center"><i>Observation: Real plume drifting over Argentina</i></td>
  </tr>
</table>

> **Technical Note:** The trajectory was also validated against **NOAA HYSPLIT** trajectory models, confirming arrival time matches for urban areas (See *Notebook 3* for the detailed technical comparison).

---
## Impact & Conclusions
The model successfully replicated the macroscopic behavior of a stratospheric eruption using limited computational resources. Key findings include:

* **Trajectory Validation:** The simulation correctly predicted the ash arrival in **San Carlos de Bariloche (~100 km distance)** in approximately **1.5 hours**, matching civil protection reports from 2015.
* **Efficiency:** Unlike complex meteorological models (Navier-Stokes) that require supercomputers, this stochastic approach runs on a standard laptop in minutes, serving as an effective "First-Order approximation" for rapid risk assessment.
* **Phenomenological Accuracy:** The inclusion of a stochastic diffusion term allowed the model to replicate the "fan-shaped" dispersion observed in NASA satellite imagery, which linear models fail to capture.

## Technical Challenges & Solutions
I moved beyond standard data analysis to implement physics simulations:

* **From Deterministic to Stochastic:** Instead of simple linear movement, I implemented the **Langevin Equation** to model atmospheric turbulence as a random walk (Wiener Process).
  
* **Vectorization:** To simulate thousands of particles efficiently, I avoided Python loops and utilized `NumPy` vectorization, calculating the state of the entire system in matrix operations.
* **Geospatial Mapping:** The mathematical output (Cartesian coordinates) was transformed into geospatial coordinates (Lat/Lon) to project the abstract physics onto a real interactive map using `Folium`.
* **Parameter Calibration:** Physical constants (column height >15km and NE trajectory) were calibrated using official eruptive chronologies from the Smithsonian Institution (GVP, 2015), ensuring the simulation reflects the real atmospheric conditions of the 2015 event.

## How to Run This Project Locally

### 1. Configuration (Simulation Parameters)
Unlike typical data analysis projects, this simulation generates its own data. You can tweak the physics in the `CONFIG` section of **Notebook 3**:

* **Wind Physics:** Adjust `VX` (Zonal wind) and `VZ` (Meridional wind) to change the plume direction.
* **Eruption Power:** Modify `ALTURA_COLUMNA` (default: 17,000m) to simulate different volcanic explosivity indices (VEI).
* **Turbulence:** Change the Diffusion Coefficient `D` to make the ash cloud more compact (lower value) or more dispersed (higher value).

### 2. Installation & Execution
1. **Clone the repository:**
   ```bash
   git clone https://github.com/FernandaVil/Volcanic-Ash-Simulation.git
2. **Navigate to the folder:**
    ```bash
      cd Volcanic-Ash-Simulation
3. **Install dependencies:**
    
    ```bash
      pip install -r requirements.txt
  (Main libraries: numpy, folium, matplotlib, scipy)
  
4. **Execution:** Open the notebooks in order.
  * 01_Ecuacion_Langevin.ipynb: Theoretical foundation and 1D tests.
  * 02_Motor_Fisico_2D.ipynb:**(Mode 1)** Development of the physics engine and risk heatmap.
  * 03_Validacion_Calbuco.ipynb: **(Mode 2)** The final geospatial simulation and validation.
## Future Improvements & Roadmap

While the current version successfully validates the physics engine against historical data, future iterations of this project could include:

* **Pipeline Integration:** Unifying the Risk Engine (Mode 1) with the Historical Calibration (Mode 2) to generate real-time risk maps for active volcanoes based on live weather API data.
* **Demographic Impact Analysis:** Overlaying the probability heatmap with population density data (using Python libraries like `geopandas`) to automatically estimate the number of affected civilians.
* **3D Visualization:** Extending the 2D Folium map to a 3D terrain model using **Cesium** or **Deck.gl** to visualize the complex interaction between the ash plume and the Andes mountain range.

## Project Structure
  ```bash
      Volcanic-Ash-Simulation/
      ├── assets/                  <-- Images and GIFs for README
      ├── output/                  <-- Generated HTML map
      ├── 01_Ecuacion_Langevin.ipynb
      ├── 02_Motor_Fisico_2D.ipynb
      ├── 03_Validacion_Calbuco.ipynb
      ├── requirements.txt
      └── README.md
 ``` 


* `3_Validacion_Calbuco.ipynb`: Main notebook with the full simulation workflow.
* `output/`: Contains the interactive mapa_calbuco_final.html.
* `requirements.txt`: Required libraries for execution.

## References & Data Sources

To ensure the physical fidelity of the simulation, parameters and validation data were sourced from official scientific repositories:

* **Satellite Imagery (Validation):**
    * **NASA Earth Observatory:** *Eruption of Calbuco Continues*. Retrieved from [science.nasa.gov](https://science.nasa.gov/earth/earth-observatory/eruption-of-calbuco-continues-85779/).
    * *Instrument:* MODIS on Terra Satellite.

* **Trajectory Data (Validation):**
    * **LALINET (Latin American Lidar Network):** *Calbuco Volcano Campaign 2015*. Retrieved from [lalinet.org](https://www.lalinet.org/campaigns/cabulco-volcano-2015).
    * *Data:* NOAA HYSPLIT Forward Trajectories (April 22-23).

* **Physical Parameters:**
  * **Global Volcanism Program, 2015:** *Report on Calbuco (Chile)* (Venzke, E., ed.). Bulletin of the Global Volcanism Network, 40:6. Smithsonian Institution. [https://doi.org/10.5479/si.GVP.BGVN201506-358020](https://doi.org/10.5479/si.GVP.BGVN201506-358020)
---
*Project developed as a personal exploration in Stochastic Modeling and Geospatial Analysis.*
