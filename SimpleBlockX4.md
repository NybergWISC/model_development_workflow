# Simple Block Assembly


#### Procedure Overview

  1. Create 4 cubes/bricks in any CAD software or download Save [SimpleBlockX4.SLDASM](SimpleBlockX4.SLDASM) and convert to ACIS format.
  2. Import file into Trelis/Cubit and inspect model for any import issues.
  3. Create the graveyard volume and define implicit complement.
  4. Finish part in Cubit/Trelis.
     * Imprint and Merge volumes
     * Assign materials
     * Export file to .h5m

Exporting the model to .h5m requires the DAGMC trelis plugin, which should be installed before starting the tutorial. Instructions are [here](https://svalinn.github.io/DAGMC/install/plugin.html).

#### Goals of Example

  * Introduce the workflow with an assembly that does not have any geometric issues.
  * Work through important processes on an example that is easy to visualize and understand.

## Procedure

### In Initial CAD program
_Any CAD engine should work for this process as long as it can export to a
format importable by SpaceClaim or Cubit/Trelis. The preferred format is ACIS (.sat)._
  1. Create four solid bricks/cubes with edges of length 100cm that are separated by 50 cm or download [SimpleBlockX4.SLDASM](https://github.com/NybergWISC/model_development_workflow/blob/master/SimpleBlockX4.SLDASM)
      * In Solidworks/Catia you can either make an assembly from 4 cubes or one part
        with 4 cubes.
      * Either will work, but depending on the ACIS exporter the program
        may ask you to convert file to part. This is fine, the volumes will remain
        separate entities in the next steps.
  2. Export the assembly/part to ACIS format (.sat file extension)
      * ACIS format 22 is preferred (default for Solidworks)
      * Use "Save As" or "Export" feature and select ACIS (.sat) format.

<details><summary> Notes </summary><p>
- The preferred format is ACIS (.sat). Note that multiple file types could be used, but there is anecdotal evidence that suggests .sat files work the best for this process. Otherwise formats such as STEP and STL are also able to be imported to Trelis/Cubit.
<br/>
- This step is unnecessary unless you want to explore the model in Solidworks or learn how to save file as .sat.
<br/>
- To skip this step download the ACIS file titled "SimpleBlockX4.sat" and move onto ANSYS SpaceClaim section.
</p>
</details>    

### In ANSYS SpaceClaim
_SpaceClaim is useful for cleaning models that have overlaps, surface conflicts,
edge contacts, and small gaps. These are further explained in the error blocks and complex model examples._
  1. The model is simple and intrinsically has no conflicts or errors so there are no
  necessary steps in SpaceClaim.

### In Cubit/Trelis
_Cubit is a meshing pre-processing software with CAD capabilities and Trelis is the commercially available version. It is used in this workflow to assign materials to volumes and export the model to the DAGMC .h5m format. Trelis and Cubit have both a GUI and a command line interface. Nearly all commands can be run through either one._

| Steps  | Graphic User Interface  | In Command Line  |
|---|---|---|
| __Import the .sat file into Trelis/Cubit__  | - Can be imported using the GUI with __File -> Import__. <br/></br> - Select the file through file explorer and open. <br/></br> - One important qualifier to include is `[attributes_on]`. | - In the command line the simple form of the command is `import acis [filepath]` <br/></br> - This command can be appended with further commands that can be found by typing `help import acis`. <br/></br> - These qualifiers correspond with the options given when importing files through the GUI. <br/></br> - One important qualifier to include is `[attributes_on]`.  |
| __How to save progress for the file as ACIS__  | - Click on file, export, and select the file destination and name. <br/></br> - Make sure that it is being exported as ACIS (.sat) and press ok to save.  | - In command line type `export acis [filepath] overwrite`.  |
| __Make sure units/distances are correct and if not, fix them__  | - A distance within the model of "X units" should have a metric distance of "X centimeters". <br/></br> - To check this click on a surface/line with a known metric value and then navigate to the properties page and check the surface area or curve length. <br/></br> - On the Command Panel click on __Geometry (leftmost option) -> Volume -> Transform -> Scale (from dropdown menu) -> Enter Scale Factor -> Apply__ | - A distance within the model of "X units" should have a metric distance of "X centimeters". <br/></br> - To check this in the command line the general form is `list [entity name and number] geom`. For example to check the geometry of volume 1 enter `list Volume 1 geom`. <br/></br> - In command line enter `vol all scale [Scale Factor]`. For example parts from solidworks would require `vol all scale 0.1` to convert the units to centimeters. |

<details><p><summary>Notes</summary>
- A more general DAGMC based tutorial for Cubit/Trelis basics can be found at https://svalinn.github.io/DAGMC/usersguide/trelis_basics.html.
<br/></br>
- "Command Panel" and "Command Line" are necessary for GUI and command line methods respectively. To enable these select View and switch "Command Line" and/or "Command Panel" to visible.
<br/></br>
- Scale will most likely be off by a factor of 10. For example models from Solidworks will most likely be increased by 10x or 100cm to 1000 units, but this scaling changes based on the initial CAD program. You must scale every entity down or up by that factor to match units to cm.
</p></details>

#### Creation of the Graveyard

_The graveyard volume is not part of the CAD geometry being analyzed, but plays a role in the final analysis. Particles stop and are tracked once they reach the graveyard volume._

| Steps  | In Graphic User Interface  | In Command Line  |
|---|---|---|
| __Create a cube large enough to envelop all other volumes.__  | - Select __Geometry -> Volume -> Create__ and select __Brick__ from the drop down menu. <br/></br> - Type 500 in each dimension box __(width, height, depth)__ and press __Apply__ to create the block.  | - The general form is `create [shape] [dimensions based on shape]`. </br> - For a simple cube 500x500x500 you can enter `create brick x 500`.  |
| __Create another slightly larger cube to be used for graveyard.__  | - In command panel create another brick in the same way as part 1, using 750 as the new dimension in each dimension box.  | - Enter `create brick x 750`  |
| __Center volumes within the larger cubes.__  | To move the blocks in command panel select __Geometry -> Volume -> Transform__ and select __Move__ from the drop down menu. <br/></br> - Type "5 6" into the __Volume ID(s)__ box. Then __Select Method -> Distance__ and type 100 into the __X Distance__ box. _This distance/direction may vary._  | - General form is `move vol [volume ids] [axis] [distance along axis]` <br/></br> - Enter `move vol 5 6 x 100`.  |
| __Subract smaller cube from larger.__  | - Select __Geometry -> Volume -> Boolean__ and select __Subtract__ from the dropdown menu. <br/></br> - Select volume 6 as volume A and volume 5 as volume B, click __Apply__.  | - Enter `subtract vol 5 from 6`|

After this step you will be left with a large, hollow cube that can be used as a graveyard volume.

<details><p><summary>Notes</summary>
- This volumes could also be created in another CAD program with the same results, this is an example of how you could create this volumes in Trelis and Cubit.
<br/></br>
- Most likely the volumes will be slightly off centered in the x direction, but the larger bricks will be centered with one another. This may vary based on the initial CAD program.
<br/></br>
- To see the blocks better you can select the wireframe option in the top toolbar. It is directly to the right of the undo arrow on the toolbar. Also you can click on the axis on the bottom left of the volume viewing area to get a straight on (x,y) view.
-
</p></details>

#### Imprinting and Merging Volumes in Cubit/Trelis
_Imprinting and Merging surfaces accelerates the DAGMC process by combining surfaces._

  1. Imprint and Merge all shared surfaces in the model.
     * If using the GUI, in command panel select __Geometry -> Volume -> Imprint Merge__ and select __Imprint and Merge__ from the dropdown menu. Then type "all" into the __Volume ID(s)__ box and click __Apply__.
     * Otherwise in command line type `imprint volume all` and then enter. Then type `merge volume all`.
     * _Now all surfaces that previously overlapped have been merged into one surface shared by both volumes._

<details><p><summary>Notes</summary>
- The default tolerated distance for imprinting/merging surfaces is 5.0e-4 units.
<br/>
- Combining surfaces cuts two surfaces and two sets of faceting areas down to one, decreasing computational power required as well as eliminating possible errors where particles exist simultaneously in two surfaces.
</p></details>

#### Assigning Materials in Cubit/Trelis and defining the Implicit Complement
_Most commands for this section are best implemented through the command line. More general instructions can
be found_ [here](http://svalinn.github.io/DAGMC/usersguide/codes/dag-mcnp.html).

_A method (implicit complement) is implemented automatically during a DAGMC run that creates a volume in all regions of space not defined by the CAD model. By default, this volume is assumed to be a vacuum, but it can be assigned to a different material as shown below. This means that while one could explicitly create void/vacuum volumes, it is most often not necessary._

  1. The general command for assigning materials and densities in Cubit or Trelis
  `group "mat:[assigned material number]/rho:[assigned density]" add
  vol [volume numbers]` where the density is in [atoms/barn-cm] for positive values and [g/cc] for negative values.
  2. The blocks will be given two different materials and then the graveyard volume will be assigned. These material assignments will depend on the material library you create/use for the project.
     * To assign one block to the first material use `group "mat:1/rho:0.0223" add vol 1`
     * To assign the rest of the blocks (volumes 2,3,4) to the second material use `group "mat:2/rho:0.03" add vol 2 to 4`
     * To assign the graveyard volume use `group "mat:Graveyard" add vol 7`
  3. To assign a non-void/vacuum material to the implicit complement you can add the graveyard to a second material group. This second material group will be applied to the implicit complement.
     * `group "mat:3/rho:0.04" add vol 7`

#### Exporting file to .h5m format
_The plugin translates the CAD file to something that can be analyzed by DAGMC. Instructions as to how to install this plugin can be found_ [here](https://svalinn.github.io/DAGMC/install/plugin.html).

1. Once the plugin is correctly installed you should receive the message `Loaded Svalinn plugin`, line break, `DAGMC export command available` in the command line.
2. Once all issues are resolved in the model, export the file as .h5m, this command must be executed through the command line.
  * The general form of the export command is `export dagmc "<filename>" [options]`. To see the options available enter `help export dagmc` into the command line.
  * The filename should include the filepath after the working directory so that it designates where the file ought to be saved after export.
  * For this part `export dagmc "Desktop/SimpleBlockX4.h5m"` will create the part as a .h5m with default settings in the Desktop.
