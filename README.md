# GDBChEMBL

GDBChEMBL is a library of 10 million chemical structures that was extracted from the exhaustively generated database GDB17.
It was generated by scoring input SMILES structures by circular substructures shared with the reference database of known chemical compounds, ChEMBL v.24.\
GDBChEMBL is available for download at http://gdb.tools/downloads/ \
The code provided with this repository was the one used for the generation of GDBChEMBL and can be used for the generation of libraries of molecular shingles of any structural database aswell as to score similarity of query structures against that database.

![molecular shingle extraction process](https://cloud.gdb.tools/index.php/apps/files_sharing/ajax/publicpreview.php?x=2493&y=938&a=true&file=mol_substruct.png&t=m23ocFkCJYW3ZEH&scalingup=0)


## Dependencies
These scripts use the rdkit.

## Deployment
Simply clone the repository and run the scripts with the desired parameters.

## Contents and Usage
* CLscore.py
* CLscore_sequential.py

These are scripts to calculate CLscore (**C**hEMBL-**L**ikeness **Score**) as done for the generation of GDBChEMBL.\
CLscore.py dynamically parallelizes computation according to available CPU cores inside the current working environment.\
CLscore_sequential.py does not parallelize or has to be parallelized using other methods.

CLscore values are written appended to their respective SMILES string.\
When using default parameters, both scripts only expect parameters for input and an output file path:
```
python CLscore.py -i $inFile -o $outFile
```

Other parameters:

| Parameter | Default | Description |
|---|---|---|
| --cutOffScore | 0.0 | Only write query structures if their CLscore is greater or equal to a given value. |
| --considerRareShingles | False | Also consider shingles that occur less than 100 times in the reference database. |
| --radius | 3 | Maximum radius of circular substructures around the rooting atom. Note that when using the ChEMBL shingle library, maximum radius will be 3 (default). For larger radii, the reference database would have to be read out accordingly. |
| --rooted | True | Use rooted shingles. This means reference shingles are canonical but always starting at the central atom of a circular substructure. False means shingles in the database are canonicalized but not rooted. It is recommended to use rooted shingles. |
| --weighted | True | Calculate score by summing up log10 of frequency of occurence inside the reference database. (default: True). When set to False, score will add 1 for every shingle inside the reference database, irrespective of how often it occurs. It is recommended to use the actual CLscore with weighted shingles. |

Example:
```
python CLscore.py -i $inFile -o $outFile --cutOffScore=3.3 --considerRareShingles=True
```

* shingle_extr_rooted_nchir.py

Script to extract and count shingles of an arbittrary set of chemical structures (SMILES) and to store it as pickled python dictionary.

To be run as follows:
```
python shingle_extr_rooted_nchir.py $inFile $outFile
```
