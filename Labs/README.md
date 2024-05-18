# Lab

<details>
<summary><strong>Basic Linux Commands</strong></summary>
<br>

1. `cd` - To navigate between the directories
2. `ls`, `ltr` - List directory contents, in long format sorted by modification time similar to 'll'
3. `ls --help` - Display help information for the `ls` command
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

Import the nesscary package to run the flow by using the following command
```
package require openlane 0.9
```

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

4. Starting the Synthesis Process
```
 run_synthesis
```
![synthesis successfull](https://github.com/Akkshayul1234/Nasscom-VSD/assets/37902660/21f98b7d-71c0-4f5c-b7cb-ab091e6d7751)

- After the Synthesis is Successfull the results are updated in the `reports/synthesis` folder.



















