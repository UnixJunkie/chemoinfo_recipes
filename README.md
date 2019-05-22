# Chemoinformatics recipes
Command line recipes for the working chemoinformatician

# Unique InChI filtering of molecules (remove duplicates)

    obabel INFILE -O OUTFILE --unique

# Assign MMFF94 partial charges

    obabel INFILE -O OUTFILE --partialcharge mmff94

# Compute MACCS 166bits fingerprints and output them as strings
# (will create a .csv file named after the input file)

    mayachemtools/bin/MACCSKeysFingerprints.pl --size 166 [INFILE] --CompoundIDMode MolName

# 3D conformer generation using Corina classic
# (one low energy conformer per molecule)
# the optional [-d wh] add/writes out hydrogens (makes them explicit)

    corina [-d wh] < INPUT.sdf > OUTPUT.sdf

# lowest energy conformer generation using cxcalc from Chemaxon

    cxcalc conformers in.smi -m 1 > out.sdf

# lowest energy conformer generation using omega from OpenEye scientific

    omega -in in.smi -out out.sdf -maxconfs 1

# RMSD between two ligands (curr. will be superposed onto ref.)

    fconv -rmsd current.mol2 --s=reference.mol2

# compute Bemis-Murcko scaffolds of molecules

    stripper --in molecules.smi --out scaffolds.txt

# print a molecule in EPS format (for LateX manuscripts); obabel then inkscape
# SMILES to EPS, MOL2 to EPS or SVG to EPS would work the same

    obabel molecule.smi -O molecule.svg
    inkscape molecule.svg -E molecule.eps --export-ignore-filters --export-ps-level=3

# smi2eps in Bash (smi -> svg -> pdf -> cropped-pdf -> ps -> eps)

```
# librsvg2-bin provides rsvg-convert
# texlive-extra-utils provides pdfcrop
# ghostscript provides pdf2ps
# ps2eps provides ps2eps
function svg2eps () {
    tmp_pdf_out=`echo $1 | sed 's/\.svg$/\_tmp.pdf/g'`
    pdf_out=`echo $1 | sed 's/\.svg$/\.pdf/g'`
    ps_out=`echo $1 | sed 's/\.svg$/\.ps/g'`
    eps_out=`echo $1 | sed 's/\.svg$/\.eps/g'`
    svg=$1
    rsvg-convert -f pdf $svg -o $tmp_pdf_out
    pdfcrop $tmp_pdf_out $pdf_out
    pdf2ps $pdf_out $ps_out
    ps2eps < $ps_out > $eps_out
}
```

```
# openbabel provides obabel
function smi2eps () {
    smi=$1
    svg_out=`echo $1 | sed 's/\.smi$/\.svg/g'`
    obabel $smi -O $svg_out -xC -xd
    svg2eps $svg_out
}
```

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

# Install rdkit on Mac OS X:

    pip3 install numpy
    brew install --HEAD rdkit --with-python3 --with-inchi

# Count molecules, works for various file formats

Store this in a 'molcount' script, somewhere on your PATH.

```
#!/bin/bash

for f in "$@"; do
    filename=`basename "$f"`
    extension="${filename##*.}"
    case "$extension" in
        mol2) egrep -c MOLECULE $f
              ;;
        plr) egrep -c '^END$' $f # position and contrib per atom to cLogP
             ;;
        pqr) egrep -c ^COMPND $f
             ;;
        sdf) grep -c '$$$$' $f
             ;;
        mol) grep -c '$$$$' $f
             ;;
        phar) grep -c '$$$$' $f # Pharao DB
             ;;
        smi) cat $f | wc -l
             ;;
        *) echo "molcount: unsupported file format: ."$f
           ;;
    esac
done
```

# Get molecules by name, for various file formats

Works even with a "database" file with millions of molecules.

lbvs_consent_mol_get from https://github.com/UnixJunkie/consent

```
lbvs_consent_mol_get -i molecules.{sdf|mol2|smi} {-names "mol1,mol2,..."|-f names_file}
```

# Sayle hashing of a molecule

Some kind of canonicalization of molecular representations, consisting in the pair:

Sayle_hash(m) = (Canonical_smile_forcing_only_single_bonds_and_noH(m), number_of_Hydrogens_on_non_carbons(m) - sum_of_formal_charges(m))

m being the molecule to hash.

# install deepchem on a Mac or Linux

No GPU support, but at least its an automatic and simple install procedure.
Deepchem's version is fixed to a version that works for what I currently do.

```
pip3 install joblib pandas sklearn tensorflow pillow simdna deepchem==2.1.1.dev353
```

# Links / Bibliography

[1] http://www.mayachemtools.org/

[2] http://openbabel.org/wiki/Main_Page
