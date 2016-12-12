# Chemoinformatics recipes
Command line recipes for the working chemoinformatician

# Unique InChI filtering of molecules (remove duplicates)
$ obabel INFILE OUTFILE --unique

# Assign partial charges using the Gasteiger-Marsili charge model (PEOE)
$ obabel INFILE OUTFILE --partialcharge gasteiger

# Compute MACCS 166bits fingerprints and output them as strings
# (will create a .csv file named after the input file)
$ mayachemtools/bin/MACCSKeysFingerprints.pl --size 166 [INFILE] --CompoundIDMode MolName

[1] http://www.mayachemtools.org/
[2] http://openbabel.org/wiki/Main_Page
