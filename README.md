# Chemoinformatics recipes
Command line recipes for the working chemoinformatician

# Unique InChI filtering of molecules (remove duplicates)
$ obabel INFILE -O OUTFILE --unique

# Assign partial charges using the Gasteiger-Marsili charge model (PEOE)
$ obabel INFILE -O OUTFILE --partialcharge gasteiger

# Compute MACCS 166bits fingerprints and output them as strings
# (will create a .csv file named after the input file)
$ mayachemtools/bin/MACCSKeysFingerprints.pl --size 166 [INFILE] --CompoundIDMode MolName

# 3D conformer generation using Corina classic
# (one low energy conformer per molecule)
# the optional [-d wh] add/writes out hydrogens (makes them explicit)
$ corina [-d wh] < INPUT.sdf > OUTPUT.sdf

# RMSD between two ligands (curr. will be superposed onto ref.)
fconv -rmsd current.mol2 --s=reference.mol2

# compute Bemis-Murcko scaffolds of molecules
stripper --in molecules.smi --out scaffolds.txt

# Install open babel from sources

    wget https://github.com/openbabel/openbabel/archive/openbabel-2-4-1.tar.gz
    tar xzf openbabel-2-4-1.tar.gz
    cd openbabel-openbabel-2-4-1/
    mkdir build
    cd build
    cat <<EOF > build.sh
    mkdir -p ~/usr
    cmake -DPYTHON_BINDINGS=true -DCMAKE_INSTALL_PREFIX:PATH=$HOME/usr ../
    EOF
    chmod 755 build.sh
    ./build.sh
    make -j4
    make install

[1] http://www.mayachemtools.org/
[2] http://openbabel.org/wiki/Main_Page
