# Lab

<details>
<summary><strong>Basic Linux Commands</strong></summary>
<br>

1. `cd` - To navigate between the directories
2. `ls`, `ltr` - List directory contents, in long format sorted by modification time similar to `ll`
3. `ls --help` - Displays the help information for the `ls` command
4. For more commands refer [Basic Linux Cmds](https://www.geeksforgeeks.org/basic-linux-commands/)

</details>

<details>
<summary><strong>Labs</strong></summary>
<br>

1. Navigate to the OpenLANE directory:
```
cd Desktop/work/tools/openlane_working_dir/openlane
```
2. Start the Docker container and to use the open lane  OpenLANE shell:
```
docker
./flow.tcl -interactive
```
![openlane startup](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/7145ed52-c1b8-46a8-9d5a-7731c14ff422)

- Import the nesscary package to run the flow by using the following command
```
package require openlane 0.9
```
- For more on OpenLANE installation and flow - [efabless repo](https://github.com/efabless/openlane)

## Setting up OpenLANE Environment

3. Prepare the design in the OpenLANE shell:
```
prep -design picorv32a
```
![design prep](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/525dce4b-8d97-4607-bd16-3c7621c8fd6a)

- The design is present in this directory
```
/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a
```
- After the design prep is completed a new `runs` folder will be created 

![runs folder](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/1878b652-9e01-4f59-8f68-5d5305039f92)

- This folder consists of the following contents.

![run folder contents](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/19f9c44b-dc6f-4b85-8d93-2a3e7d0eac7f)

## Synthesis 

- Command:
```
 run_synthesis
```
![synthesis successfull](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/21f98b7d-71c0-4f5c-b7cb-ab091e6d7751)

- After the Synthesis is Successfull the results are updated in the `reports/synthesis` folder.

![flop ratio](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/3d48b9cf-9169-4c70-933a-68373ce215a3)

- From this we can calculate the
```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}  = \frac{1613}{14876} = 0.1084
```
- In terms of % = Flop Ratio * 100 = `10.84 %`

## Floorplan (FP)

- Before running the FP, if we want to add any switches or change any values we should add these in `designs/picorv32a/sky130A_sky130_fd_sc_hd_config.tcl` as this is the pdk specific file that overrides the normal `config.tcl` and also the `floorplan.tcl` file.

- Command:
  ```
  run_floorplan
  ```
  ![floorplan successfull](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/6b135085-ff4d-470d-b4e7-261958afdc52)

  - After the floorplan is sucessfull we get these files in results directory.

    ![floorplan results](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/1d5cca4b-65b4-490d-b8ff-9223a4999135)

  - `picorv32a.floorplan.def.png`

    ![picorv32a.floorplan.def.png](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/d9a7af80-457d-436e-ae6b-912e34b36082)

  - inside the `.def` file, we have the info that can help us in measuring the size of the die

    ![def file](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/680559bf-6378-4440-8428-ddcc87248af7)

```math
Given\ 1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width = 660685 \ in \ unit \  distance
```
```math
Die\ height = 671405 \ in \ unit \  distance
```

```math
Die\ width = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die = 660.685 * 671.405 = 443587.212425\ Sq\ Microns
```
## MAGIC
- Magic is a layout tool that helps us to view the chip layout of what we created in the floorplanning stage
- Invoking the magic tool, requires the `lef` & `def` files along with the PDK's `.tech` file in order to map and view the floorplan in magic.
  
  ```
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
  ```
- Zoomed in view of the layout
  ![zoomed in voew](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/a7b9c740-c8bd-40f8-a6e5-144376ba1aba)

- To view any cell hover over the cell and click `s` and then type `what` on the `tkcon`
  ![what on tkcon](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/370c3942-4704-48ce-b523-a9d3f6dc7cec)

- Vertical cells at the bottom
  ![vertical cells](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/847c478a-9fa8-4c36-9475-dc868a57541a)






  




















