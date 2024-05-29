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

## PLACEMENT

- Command
  ```
  run_placement
  ```
  ![placement done](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/b97856de-efc1-47f1-98d8-6ba2f119167d)

  ![slack](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/17fa2371-d8ee-47a7-b35a-ee2f92823a9f)

  `picorv32a.placement.def.png`

  ![picorv32a.placement.def.png](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/8b372a8f-5781-4742-aee4-841f743a6805)

- If we look at the log file we see all the values of the Placement Analysis.
  
  ![placement analysis](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/fb00ac7a-b34d-4f70-a0b6-c4b1bb1741e8)

- Magic View of Design

  ![magic view](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/f9911fe7-7c25-46a4-8605-9e54eb73d5c6)

- Zoomed view
  
  ![placement zoomed view](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/9fcde103-66cf-41a4-9d13-2a4ef0bd7a1d)

## LIBRARY CELL DESIGN USING MAGIC AND NGSPICE

- We are going to see how to create the std cell layout and extract it using ngspice
- The First step is to clone the [Github Repo](https://github.com/nickson-jose/vsdstdcelldesign) into our openlane directory
  
  ```
  git clone https://github.com/nickson-jose/vsdstdcelldesign.git
  ```
  ![repo clone](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/5d590ae3-9521-4f75-9e8a-e8bf66e26e92)

- Copy the `Sky130A.tech` from `openlane_working_dir/pdks/sky130A/libs.tech/magic` to the `vsdstdcelldesign` folder.

  ![vsdstdcell contents ](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/8c39c9b5-b0d6-4ba1-9485-f6ba41c5e6ce)
  
- Open the `CMOS inverter` and the `Sky130A`by using magic
  ```
   magic -T sky130A.tech sky130_inv.mag &
  ```
  ![cmos inverter](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/744da879-7cd0-4faa-a305-9d12fa8b2b0a)

- You can see the various diffusion, metal lasyers, nmos and pmos in the design by hovering over them and pressing `s` to select and typing `what` in the `tkcon` window.

  ![layers of the inverter](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/fc9eeaef-9f40-4864-86d6-24ce4545ba73)

  ![nmos pmos](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/eb3bef97-5976-4cfa-9896-c8780d9c53e2)

## EXTRACTING THE SPICE NETLIST
- Below commands are used to extract the spice netlist, this done while `design` is open in `magic`.
  ```
  extract all
  ext2spice cthresh 0 rthresh 0
  ext2spice
  ```
![extarcting spice](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/a31b4321-25dd-4563-ae59-1c9c64bcff58)

- In `vsdstdcelldesign` folder we will have the extracted spice netlist in a `.spice` file

![spice netlist](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/85d04f5e-1b86-49dc-8b7f-3e0dbc075369)

- In this we have to modify the following:
   - include pshort nshort library files 
   - define VDD and VSS sources
   - create a pulse signal to create transient analysis

![spice netlist edited](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/66d876f3-e2a9-4fed-bf25-0bf351bfc6f1)

- To view the `Transient Analysis Graph` of the inverter we invoke the ngspice tool
  ```
  ngspice sky130_inv.spice
  ```
![ngspice tool](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/6563b26f-637f-4816-8543-fd64ee0453ed)

- To plot the graph we use type `plot Y vs time a` in the `ngspice` terminal

![graph](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/3f602fda-ef4e-43ea-8d28-e1150e1ce26f)

![20% value](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/eb7fa6f5-a234-4baa-9cd3-5f5f14ed4516)

![80% value](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/4a91fbc4-6aaa-4939-b73f-1bc0bedce07d)
  
**Rise Time**

![rise time](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/21a9eafe-09c3-4174-b95d-926fe332bebe)
- Rise time transition 20% to 80% - time value = (2.245-2.182)e-09 = 63ps
  
**Fall Time**

![image](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/f2c1adc6-1efa-46a1-a5e3-edbe7239f800)
- Fall time transition 80% to 20% - time value = (4.096-4.058)e-09 = 38ps

**Cell Rise Delay**

![cell rise delay](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/b2eaba33-71e2-4dbb-a428-7647f2f3c764)
- Calculated at 50% of VDD (2.210-2.149)e-09 = 61ps

**Cell Fall Delay**

![cell fall delay](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/384ab243-4800-41ba-9e82-cd59466b949c)
- Calculated at 50% of VDD (4.077-4.049)e-09 = 28ps


  

  

 


  
  







  




















