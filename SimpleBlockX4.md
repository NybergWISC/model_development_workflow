# Creation of Simplest Blocks

#### Description of Final Product

  * Four cubes/bricks created in any general CAD software ready for DAGMC. Meshed with "Tetmesh"

#### Goals of Example

  * Introduce the workflow with an assembly that does not have any geometric issues
  * Work through important processes on an example that is easy to visualize and understand.

## Procedure Overview

#### In Initial CAD program
_Any CAD engine should work for this process as long as it can export to a
format importable by SpaceClaim or Cubit/Trelis. The preferred format is ACIS (.sat)._
  1. Create four solid bricks/cubes with edges of length 100cm that are separated by 50 cm
    * In Solidworks/Catia you can either make an assembly from 4 cubes or one part with 4 cubes
      - Either will work, but depending on the ACIS exporter the program may ask you to convert file to part. This is fine, the volumes will remain separate entities in the next steps.
  2. Export the assembly/part to ACIS format (.sat file extension)
    * ACIS format 22 is preferred (default for Solidworks)
    * Note that multiple file types could be used, but there is anecdotal evidence that suggests .sat files work the best for this process. Otherwise formats such as STEP and STL are also able to be imported to Trelis/Cubit and SpaceClaim.
    * Use save or export feature and select ACIS (.sat) format

#### In ANSYS SpaceClaim
_SpaceClaim is useful for cleaning models that have overlaps, surface conflicts,
edge contacts, and small gaps. These are further explained in the error blocks example._
  1. Since the model intrinsically has no conflicts or errors there are no
  necessary steps in SpaceClaim.

#### In Cubit/Trelis
_Cubit and Trelis are basically the same CAD software with Trelis being the commercial
version. They are most useful for the meshing of geometries and assigning materials to volumes.
They have both a GUI and a command line interface. Nearly all commands can be run through
both. For each step the GUI will be explained first, and the command line commands given second. "Command Panel" and "Command Line" are necessary for GUI and command line methods respectively. To enable these got to_ __View__ _and switch "Command Line" and/or "Command Panel" to visible._

  1. Import the .sat file into Trelis/Cubit
    * Can be imported using the GUI with __File -> Import__. Select the file through file explorer and open. There are further options to select from. For descriptions of these options you can click on the question box on the bottom left, but they will not affect the outcome of this particular import.
    * In the command line the simple form of the command is "import acis < insert filepath >" This command can be appended with further commands that can be found in __Help -> Geometry -> Importing -> Importing ACIS files__. These qualifiers correspond with the options given when importing files through the GUI
  2. Set the sizing factor to prepare part for meshing
    * In the command panel select the __Mode -> Mesh__ (purple cube), __Entity -> Volume__ (purple cube), and __Action -> Intervals__ (square with dimensioning lines). Type "1 2 3 4" into the select volumes box to select the 4 cubes for setting intervals. Auto factor with the default coarseness is acceptable so hit "Apply".
    * For command line type "volume 1 2 3 4 size auto factor 5"
      - This command creates an auto sized mesh with the default/medium coarseness
      - Typing "volume 1 2 3 4 size #" would select the first four volumes as listed in the model tree and set the size of any future mesh to # units.
      - Note that "1 2 3 4" could be replaced by "1 to 4" for all instances of selecting volumes.
  3. Mesh files with tetmesh
    * In the command panel directly to the right of the "Intervals" action is the "Mesh" action. Type "1 2 3 4" into the box under "Select Volumes" or click the box and hold the control key while selecting all of the cubes. Then select "Tetmesh" from the dropdown menu under "Select Meshing Scheme" and click mesh.
      - You may want to explore how the options for this action affect the final mesh, but default settings are fine for our purposes.
    * To mesh through command line you must set the mesh scheme to Tetmesh and then mesh separately. To set the scheme to Tetmesh type "volume 1 2 3 4 scheme tetmesh" then after that command is complete simply type "mesh volume 1 2 3 4" and the cubes will be successfully meshed with Tetmesh.
  4. Save the file as ACIS
    * Click on file, export, and select the file destination and name. Then make sure that it is being exported as ACIS (.sat) and press ok to save.
    * In command line type "export acis < filepath > overwrite".
