# Volcanic Ash Dispersion Simulation via Stochastic Differential Equations

This project develops a physics-based computational model to simulate the atmospheric transport of volcanic ash using **Langevin Stochastic Differential Equations (SDEs)**. While the simulation uses the **2015 Calbuco Eruption (Chile)** as a validation case study, the engine is designed to be adaptable to any emission source by modifying physical parameters (wind vectors, height, and diffusion coefficients).

> 🇪🇸 [Versión en Español](./README.es.md)
>
## Visual Results (Case Study: Calbuco 2015)
![Simulation Demo](./assets/demo_calbuco.gif)
*Lagrangian particle simulation visualizing the first 5 hours of the eruption. Red particles indicate the active plume front, while yellow represents lower-density dispersion driven by stochastic turbulence.*

### Validation: Model vs. Historical Data
To verify the physical accuracy of the simulation, results were compared against the **NOAA HYSPLIT** atmospheric model for the same date (April 22, 2015).

<table>
  <tr>
    <td align="center"><b>Stochastic Simulation (Python)</b></td>
    <td align="center"><b>NOAA HYSPLIT Model (Reference)</b></td>
  </tr>
  <tr>
    <td align="center"><img src="./assets/simulacion_zoom_out.png" width="400"></td>
    <td align="center"><img src="./assets/hysplit_crop.jpg" width="400"></td>
  </tr>
  <tr>
    <td align="center"><i>Result: Northeast trajectory (NE)</i></td>
    <td align="center"><i>Result: 15km height trajectory (Green Line)</i></td>
  </tr>
</table>

## Impact & Conclusions
The model successfully replicated the macroscopic behavior of a stratospheric eruption using limited computational resources. Key findings include:

* **Trajectory Validation:** The simulation correctly predicted the ash arrival in **San Carlos de Bariloche (~100 km distance)** in approximately **1.5 hours**, matching civil protection reports from 2015.
* **Efficiency:** Unlike complex meteorological models (Navier-Stokes) that require supercomputers, this stochastic approach runs on a standard laptop in minutes, serving as an effective "First-Order approximation" for rapid risk assessment.
* **Phenomenological Accuracy:** The inclusion of a stochastic diffusion term allowed the model to replicate the "fan-shaped" dispersion observed in NASA satellite imagery, which linear models fail to capture.

## Technical Challenges & Solutions
I moved beyond standard data analysis to implement physics simulations:

* **From Deterministic to Stochastic:** Instead of simple linear movement, I implemented the **Langevin Equation** to model atmospheric turbulence as a random walk (Wiener Process).

  $$d\mathbf{x} = (\mathbf{u} + \mathbf{v}_{term})dt + \sqrt{2D} d\mathbf{W}_t$$

* **Vectorization:** To simulate thousands of particles efficiently, I avoided Python loops and utilized `NumPy` vectorization, calculating the state of the entire system in matrix operations.
* **Geospatial Mapping:** The mathematical output (Cartesian coordinates) was transformed into geospatial coordinates (Lat/Lon) to project the abstract physics onto a real interactive map using `Folium`.
* **Parameter Calibration:** The model required tuning the wind vectors ($v_x, v_z$) based on historical vector analysis to match the specific Northeast trajectory of the 2015 event.

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
  * 02_Motor_Fisico_2D.ipynb: Development of the physics engine.
  * 03_Validacion_Calbuco.ipynb: (Main) The final geospatial simulation and validation.

## Project Structure
  ```bash
      Volcanic-Ash-Simulation/
      ├── assets/                  <-- Images and GIFs for README
      ├── output/                  <-- Generated HTML map
      ├── 1_Ecuacion_Langevin.ipynb
      ├── 2_Motor_Fisico_2D.ipynb
      ├── 3_Validacion_Calbuco.ipynb
      ├── requirements.txt
      └── README.md
 ``` 


* `3_Validacion_Calbuco.ipynb`: Main notebook with the full simulation workflow.
* `output/`: Contains the interactive mapa_calbuco_final.html.
* `requirements.txt`: Required libraries for execution.

---
*Project developed as a personal exploration in Stochastic Modeling and Geospatial Analysis.*
