## gninaworkflow

This Jupyter Notebook based workflow is used to process proteins and ligands as preparation for Gnina docking. 

# Executing the workflow to dock ligands using Gnina

0. Install Gnina and dependencies
   1. Follow the relevant set of installation instructions provided by the authors of Gnina: https://github.com/gnina/gnina.
   2. Install the following:
      1. Jupyter Notebook
      2. PDBFixer

1. Process your chosen protein files and generate docked ligand .sdf files using /proteinprep01/proteinprep.ipynb. These protein complex files require available .pdb files that contain docked ligands. This code will output a 'docked' ligand as an .sdf file which will be used to define the binding location of the protein. This code will also create a PDBFixer 'fixed' .pdb file of the protein that is ready for Gnina docking.
   1. Edit proteinprep.ipynb to define list of proteins to process. This step requires the definition of the protein chain that contains the docked ligand, the code for the docked ligand, and the location of the .pdb file for download. It is up to the user to determine the best chain and appropriate ligand code for each specific protein.
   2. Evaluate the configuration for PDBFixer to verify it is appropriate for your use case.
   3. Execute proteinprep.ipynb in Jupyter notebook which will write out new processed .sdf and .pdb files along with a .json file that can be later ingested by dock.ipynb.
	
2. Process chosen ideal ligands with /dock05/acquire-minimize.ipynb. This file opens one or many .csv files, looks for CAS numbers, writes a master CAS file, reads through the master file, gets file metadata from PubChem, downloads a 3d .sdf from PubChem to local filesystem, then creates a miminimized version of .sdf in a separate folder.
   1. Edit the configuration section of acquire-minimize.ipynb to point to one or many .csv files that define desired ideal ligands for docking with Gnina. These .csv files should contain the CAS numbers for the selected ideal ligands.
   2. Execute the 'download' block and take note of failed files. Each of these failed files will need to be downloaded one by one as demonstrated by an example in the 'Download missed files' block.
   3. After resolving missed files execute the 'Minimize the SDF files' block to finalize the minimization process.

3. Perform docking simulations via Gnina and report results to screen and/or filesystem. 
   1. Review configuration options within dock05/config.py and dock05/dock.ipynb
   2. Execute blocks within dock05/dock.ipynb that load and display ligands and protein files as necessary. Each block has notes that describe the purpose and when and why they are needed. Some are optional.
   3. Examine 'Define Gnina' block to see and possibly modify gnina parameters.
   4. Execute blocks within the 'Run the docking simulations' section. The block that calls Gnina is computationally expensive. Expect about 30 seconds per docking run depending on processing resources.
   5. There are multiple blocks that allow the results to be sorted by different columns. The final block writes the results to the filesystem.
