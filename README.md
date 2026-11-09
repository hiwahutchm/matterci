
- Modelica Association: [modelica/MAP-LIB_ReferenceResults](https://github.com/modelica/MAP-LIB_ReferenceResults)
- PNLib: [AMIT-HSBI/PNlib](https://github.com/AMIT-HSBI/PNlib)
- DLR-SR: [DLR-SR/ThermoFluidStream-Regression](https://github.com/DLR-SR/ThermoFluidStream-Regression)
  and [DLR-SR/PlanarMechanics_ReferenceResults](https://github.com/DLR-SR/PlanarMechanics_ReferenceResults)

To download the MSL reference files create a file
installReferenceResults.sh with

```sh
#!/bin/sh

refdir="/some/path/to/ReferenceFiles"   # Change the path!

# Update git repo for MSL Reference files
mkdir -p $refdir/modelica.org/ReferenceResults
cd $refdir/modelica.org/ReferenceResults
rm -rf $refdir/MAP-LIB_ReferenceResults/

test -f MAP-LIB_ReferenceResults.git/config || git clone --bare https://github.com/modelica/MAP-LIB_ReferenceResults.git MAP-LIB_ReferenceResults.git
cd MAP-LIB_ReferenceResults.git
git fetch origin '*:*'
git fetch --tags --force

for tag in $(git for-each-ref --format="%(refname:lstrip=-1)" refs/heads/)
do
  echo "tag: $tag"
  base="$refdir/MAP-LIB_ReferenceResults/$tag"
  mkdir -p $base
  echo "mkdir -p $base"
  git archive --format=tar $tag | (cd $base && tar xvf -)
done
```

and run it

```bash
chmod a+rx installReferenceResults.sh
./installReferenceResults.sh
export MSLREFERENCE="/some/path/to/ReferenceFiles/"
```

For the other libraries just clone the repositories to `/some/path/to/ReferenceFiles/`.
