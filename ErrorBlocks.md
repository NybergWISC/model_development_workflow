# Assembly of Blocks with Errors

#### Description of Final Product

  * Four cubes built from .sat file with issues resolved in SpaceClaim
    - All issues resolved using SpaceClaim
    - Volumes all assigned materials
    - Exported as .h5m file
  * Void volume (with creation example in both Cubit/Trelis and Solidworks)

#### Goals of Example

  * Execute the workflow with a model that has geometric issues to be resolved in SpaceClaim.
  * Work through important processes on an example that is easy to visualize and understand.
  * Provide a general understanding of what issues could arise on more complex models.

## Procedure Overview

### In Initial CAD program
_Any CAD engine should work for this process as long as it can export to a
format importable by SpaceClaim or Cubit/Trelis. The preferred format is ACIS (.sat)
This step is unnecessary unless you want to explore the model in Solidworks or learn how to save file as .sat. To skip this step download the ACIS file titled "ErrorBlocks.sat" and open as described in ANSYS section._
  1. Download the part titled "ErrorBlocks.sw" from this page and open in Solidworks.
  2. Export the assembly/part to ACIS format (.sat file extension)
     * ACIS format 22 is preferred (default for Solidworks)
     * Note that multiple file types could be used, but there is anecdotal evidence that suggests .sat files work the best for    this process. Otherwise formats such as STEP and STL are also able to be imported to Trelis/Cubit.
     * Use save or export feature and select ACIS (.sat) format

### In ANSYS SpaceClaim
_SpaceClaim is useful for cleaning models that have small gaps, overlaps, edge contacts, and unnecessary geometric detail. For all of these edits one must work closely with the creator of the model/experiment to determine which
complications are important to the end result and which are able to be changed. More general SpaceClaim tutorials/support can be found_ [here](http://www.spaceclaim.com/en/Support/Tutorials/Essentials.aspx).
  1. Open ErrorBlocks.sat by clicking file, open, and then sorting by .sat type files.
  2. Basic issues with the model through which particles could be lost during analysis.
      * Gaps: when particles encounter gaps they may lose track of which volume they are in and be lost. In this model there is one obvious gap, but for real models gaps are only an issue when the gap is around 10^-6 units (cm). In many cases small gaps can also be fixed by the imprint/merge step in Trelis/Cubit This gap is made larger in this example so that it can be found visually as well as with the SpaceClaim tool.
      * Overlaps: when particles encounter overlaps between two volumes they may not be able to determine which volume they are in and be lost. There is one volume that is obviously overlapping with other volumes.
      * Edge contacts: there is a possibility that when two volumes are only in contact on a single edge it can cause an issue with surface definitions. There is one example of an edge contact in this model.
      * Unnecessary Detail: all details in the CAD model are included in the meshing process and in the final analysis and small details can greatly increase the amount of computation necessary. The physical threads included in this model are one example. This issue is a problem of optimization and depends on the computing power available as well as the level of detail necessary.
  3. Identify all basic issues which exist within the model
      * Visually:
        - There is one instance of each basic issue all of which are relatively conspicuous and easy to visually find.
        - Most often unnecessary detail must be determined intuitively and visually while overlaps, edge contacts, and small gaps cannot be found visually.  
      * With SpaceClaim tools:
        - Gaps can be identified by navigating to the "Prepare" tab and selecting "Clearance" under the "Detect" section. In this case to find the gap enter a maximum distance of `1cm` in the "Find Options" box. The gap will be highlighted in red.
        - Overlaps can be identified by navigating to the "Prepare" tab and selecting "Interference" under the "Remove" section. There are technically two volumes of interference and both will be highlighted in red.
        - The "Imprint" tool highlights any coincident edges or faces within the part and can make it easier to find edge contacts as well as overlaps. This tool can be found under the "Prepare" tab and in the "Analysis" section.
        - There are no built-in tools in SpaceClaim to generally identify unnecessary details.  
  4. Fix the basic issues with the tools available in SpaceClaim.
      * Note that the order in which you fix issues matters. If you were to fix the overlap and then the gap, there would be an extra step because fixing the gap would naturally create another overlap.
      * Gaps: To fix gaps oftentimes the best option is to use the "Pull" tool and fix any possible overlaps later.
        - First select one of the two faces on either side of the gap using the "Select" tool. To make it easier to see the face you can hide one of the two components by unchecking the box under the "Structure" toolbar.
        - Next click the "Pull" tool and then select the "Up To" option and click any face of the other component that borders the gap. This will extend one of the two components to fill the gap.
      * Overlaps: The "Interference" tool gives the option to subtract an interfering/overlapping region from one of the two bodies.
        - The choice of which to volume to subtract from may be important based on the materials of each volume so be sure to note which component it subtracts from.
        - SpaceClaim automatically selects the larger component as the one to subtract from. To switch to the other volume simply check the box labeled "Subtract from smaller body" located in Fix Options.
      * Edge contacts: There are two main ways to resolve edge contacts, the first is to create a wider edge so that there is a common surface, the second is to cut out the edge so that there is are no shared features between the two components. This is dependent on the nature of the model and the analysis.
        - Go to the design tab, sketch section, and select the line option. Create a rectangle that is perpendicular to the edge contact and shares an edge with the top edge of the face that contains the edge contact.
        - Select the rectangle using the "Select" tool and then click "Pull".
          * To create a gap select "Cut" from the "Options-Pull" toolbar and then either drag the cut to the bottom of the component, use the "Up To" option and select the bottom face, or use the "Ruler" option and make the pull 100cm.
          * To create a wider edge select "Add" and -similarly to cutting- either drag the cut to the bottom of the component, use the "Up To" option and select the bottom face, or use the "Ruler" option and make the pull 100cm. This method will create interference between the new volume and old which can be resolved using the "Interference" tool.
      * Unnecessary detail: In this particular case the physical threads are an unnecessary detail as they will cause a significant amount of computational load, but have very little effect on the outcome of any significant result. Which details are important to keep and which to delete is dependent on the nature of the model and the analysis. These features can be adjusted with whatever software is easiest for each user.      


### In Cubit/Trelis
_These steps have been previously explained in SimpleBlockX4.md. Cubit and Trelis are basically the same CAD software with Trelis being the commercial
version. They are most useful for the meshing of geometries and assigning materials to volumes.
They have both a GUI and a command line interface and nearly all commands can be run through
both. For each step the GUI will be explained first, and the command line commands given second.
"Command Panel" and "Command Line" are necessary for GUI and command line methods respectively.
To enable these select_ __View__ _and switch "Command Line" and/or "Command Panel" to visible.
A more general DAGMC based tutorial for Cubit/Trelis basics can be found_ [here](https://svalinn.github.io/DAGMC/usersguide/trelis_basics.html).

  1. Import the .sat file into Trelis/Cubit
     * Can be imported using the GUI with __File -> Import__. Select the file through file explorer and open. There are further options to select from. For descriptions of these options you can click on the question box on the bottom left, but they will not affect the outcome of this particular import.
     * In the command line the simple form of the command is `import acis [filepath]` This command can be appended with further commands that can be found in __Help -> Geometry -> Importing -> Importing ACIS files__. These qualifiers correspond with the options given when importing files through the GUI. One important qualifier to include is `[attributes_on]`.
  2. How to save progress for the file as ACIS
     * Click on file, export, and select the file destination and name. Then make sure that it is being exported as ACIS (.sat) and press ok to save.
     * In command line type `export acis [filepath] overwrite`.
  3. Make sure units are correct and if not, fix them
     * One interesting issue with Cubit/Trelis and the .sat file system is that distances are not always preserved when imported.
     * Cubit/Trelis does not explicitly define units in terms of inches or centimeters, but instead uses just "units". A distance within the model of "X units" should have a metric distance of X centimeters.
       - To check this click on a surface/line with a known metric value and then navigate to the properties page and check the surface area or curve length.
       - To check this in the command line the general form is `list [entity name and number] geom`. For example to check the geometry of volume 1 enter `list Volume 1 geom`. This will also give other useful information such as the full bounding box, surfaces contained, and centroid.
     * Scale will most likely be off by a factor of 10. For example models from Solidworks will most likely be increased by 10x or 100cm to 1000 units, but this scaling changes based on the initial CAD program. You must scale every entity down or up by that factor to match units to cm.
       - On the Command Panel click on __Geometry (leftmost option) -> Volume -> Transform -> Scale (from dropdown menu) -> Enter Scale Factor -> Apply__
       - In command line enter `vol all scale [Scale Factor]`. For example parts from solidworks would require `vol all scale 0.1` to convert the units to centimeters.
  4. Inspect models and resolve overlapping volumes
     * In command line one can fix overlaps by typing `validate vol all` and then `autoheal problem vols`. This should work to detect overlaps, but is not as robust as SpaceClaim.

#### Creation of void/Vacuum Volume and Graveyard
_The vacuum and graveyard are volumes that are not part of the model being analyzed, but play a role in the final analysis. A method is implemented automatically during a DAG-MCNP5 run that creates a void volume in all spaces without geometry. This means that while one can explicitly create void volumes, it is often not necessary. These volumes could also be created in another CAD program and then assigned a material in the exact same way, but this is an example of how to create these volumes in Trelis and Cubit._

  1. Create a cube large enough to envelop all other volumes.
     * If you already adjusted the scale of the objects they should be 100 units on every side with 50 units between them. A cube with side length of 500 works for these purposes
     * In command panel select __Geometry -> Volume -> Create__ and select __Brick__ from the drop down menu. Type 500 in each dimension box __(width, height, depth)__ and press __Apply__ to create the block.
     * For command line the general form is `create [shape] [dimensions based on shape]`. For a simple cube 500x500x500 you can type `create brick x 500`.
  2. Create another slightly larger cube to be used for graveyard.
     * The graveyard must be a volume, but the size does not matter, only that it fully surrounds the volumes. For this we will use a 750 unit brick.
     * In command panel create another brick in the same way as part 1, using 750 as the new dimension in each dimension box.
     * In command line type `create brick x 750`
  3. Center volumes within the larger cubes.
     * To better view where the volumes are within the larger cubes select the wireframe option in the top toolbar. It is directly to the right of the undo arrow on the toolbar.
     * Most likely the volumes will be slightly off centered in the x direction, but the larger bricks will be centered with one another. This may vary based on the initial CAD program.
     * To see the blocks better you can click on the axis on the bottom left of the volume viewing area to get a straight on (x,y) view.
     * To move the blocks in command panel select __Geometry -> Volume -> Transform__ and select __Move__ from the drop down menu. Type "5 6" into the __Volume ID(s)__ box. Then __Select Method -> Distance__ and type 100 into the __X Distance__ box. This distance/direction may vary.
  4. Hollow out large cubes to match functionality.
     * In command panel select __Geometry -> Volume -> Boolean__ and select __Subtract__ from the dropdown menu. First select volume 6 as volume A and volume 5 as volume B. Check __Keep Originals__ and click __Apply__. You can then delete volume 6 by selecting __Geometry -> Volume -> Delete__ and typing 6 into the __Volume ID(s)__ box and clicking apply.
     * In command line type `subtract vol 5 from 6 keep` Then type `delete vol 6` to get rid of the original large box.
     * After these steps you will be left with a large hollow cube that will be used for the graveyard. Next we will create the void volume in much the same way.
     * In command panel select __Geometry -> Volume -> Boolean__ and select __Subtract__ from the dropdown menu. Select volume 5 as volume A and volumes 1,2,3, and 4 as volume B. Again make sure to check __Keep Originals__ and click __Apply__. Then use the delete action to delete volume 5.
     * In command line type `subtract vol 1 2 3 4 from 5 keep` or `subtract vol 1 to 4 from 5 keep`. Then type `delete vol 5`.
     * Now you are left with a brick void volume with cutouts around the inner volumes and a graveyard volume surrounding everything else.

#### Imprinting and Merging Volumes in Cubit/Trelis
_Imprinting and Merging surfaces accelerates the DAGMC process by combining surfaces and simplifying the process so that no area in space can be owned by more than one volume. The default tolerated distance for imprinting/merging surfaces is 5.0e-4 units._

  1. Imprint and Merge all shared surfaces in the model.
     * In command panel select __Geometry -> Volume -> Imprint Merge__ and select __Imprint and Merge__ from the dropdown menu. Then type "all" into the __Volume ID(s)__ box and click __Apply__.
     * In command line enter `imprint volume all`. After that process has finished enter `merge volume all`.
     * Now all surfaces that previously overlapped have been merged into one surface shared by both volumes.

#### Meshing in Cubit/Trelis
_Meshing is completed automatically when using the_ `export dagmc` _command, but you may want to find errors in meshing beforehand or if you require unstructured mesh tallies. More information on unstructured mesh tallies can be found_ [here](https://svalinn.github.io/DAGMC/usersguide/tally.html). _The originally intended use of Cubit and Trelis was to mesh models for analysis. DAGMC uses "Tetmesh", a triangular mesh, to perform analysis. Overly complex and unnecessary features may make the meshing process overly time and computationally intensive. This is one process that simplifications in SpaceClaim help._

  1. Set the sizing factor to prepare part for meshing
     * In the command panel select the __Mesh (purple cube)-> Volume -> Intervals (square with dimensioning lines)__. Type `all` into the select volumes box to select all of the volumes for setting intervals. Auto factor with the default coarseness is acceptable so hit "Apply".
     * For command line type `volume all size auto factor 5`
       - This command creates an auto sized mesh with the default/medium coarseness
       - Typing `volume all size [factor size]` would select the first four volumes as listed in the model tree and set the size of any future mesh to # units.
     * Note that one could select smaller subsets of volumes and mesh each at different levels of coarseness. This may be useful if some volumes require more/less precision than others.
  2. Mesh files with tetmesh
     * In the command panel directly to the right of the "Intervals" action select the "Mesh" action. Type "all" into the box under "Select Volumes" or click the box and hold the control key while selecting all of the cubes. Then select "Tetmesh" from the dropdown menu under "Select Meshing Scheme" and click mesh.
     * To mesh through command line you must set the mesh scheme to Tetmesh and then mesh separately. To set the scheme to Tetmesh type `volume all scheme tetmesh` then after that command is complete simply type `mesh volume all` and the cubes will be successfully meshed with Tetmesh.
     * You may want to explore how the options for this action affect the final mesh, but default settings are fine for our purposes. Now all surfaces are described by triangular meshes that will be used during analysis.

#### Assigning Materials in Cubit/Trelis
_Most commands for this section are best implemented through the command line. More general instructions can
be found_ [here](http://svalinn.github.io/DAGMC/usersguide/codes/dag-mcnp.html).

  1. The general command for assigning Materials/Densities in Cubit/Trelis
  `group "mat:[assigned material number]/rho:[assigned density]" add
  vol [volume numbers]` where the density is in [atoms/barn-cm] for positive values and [g/cc] for negative values.
  2. The blocks will be given two different materials and then the void and the graveyard volumes will be assigned. These material assignments will depend on the material library you create/use for the project.
     * To assign one block to the first material use `group "mat:1/rho:0.0223" add vol 1`
     * To assign the rest of the blocks to the second material use `group "mat:2/rho:0.03" add vol 2 to 4`
     * To assign the void volume to vacuum use `group "mat:Vacuum" add vol 5`
     * Finally to assign the graveyard volume use `group "mat:Graveyard" add vol 6`

#### Exporting file to .h5m format
_This plugin translates the CAD file to something that can be analyzed by DAGMC. Instructions as to how to install this plugin can be found_ [here](https://svalinn.github.io/DAGMC/install/plugin.html).

  1. Once the plugin is correctly installed you should receive the message `Loaded Svalinn plugin`, line break, `DAGMC export command available` in the command line.
  2. Once all issues are resolved in the model, export the file as .h5m, this command must be executed through the command line.
     * The general form of the export command is `export dagmc "<filename>" [options]`. To see the options available enter `help export dagmc` into the command line.
     * The filename should include the filepath after the working directory so that it designates where the file ought to be saved after export.
     * For this part `export dagmc "Desktop/ErrorBlocks.h5m"` will create the part as a .h5m with default settings in the Desktop.
