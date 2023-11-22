# danesfield


# DATA FILES

https://kitware.github.io/vtk-js-datasets/list/jacksonville.html
http://138.231.80.166:2334/core3d/
https://www.businesslocationcenter.de/downloadportal




# VTK install

mkdir -p ./vtk
git clone --recursive https://gitlab.kitware.com/vtk/vtk.git ./vtk/source

mkdir -p ./vtk/build
cd ./vtk/build
cmake -GNinja ../source

cmake --build ./


# TEST SCRIPT

#!/bin/bash

tiler()
{
    #PYTHONPATH="${SCRIPT_DIR}/.." "${VTK_DIR}/bin/vtkpython" "${SCRIPT_DIR}/tiler.py" "$@"
    "${VTK_DIR}/vtkWrapPython-9.3" "${SCRIPT_DIR}/tiler.py" 
}

# generate 3D Tiles
CITY=jacksonville
#SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )
SCRIPT_DIR=~/workspace/gisconverter/Danesfield/tools
DATA_DIR=~/workspace/gisconverter/data
VTK_DIR=~/workspace/gisconverter/vtk/build/bin
echo "$SCRIPT_DIR" "$DATA_DIR" "$VTK_DIR"

rm -rf "${CITY}"
mkdir "${CITY}"

CMD=tiler "${DATA_DIR}"/CORE3D/jacksonville/jacksonville.obj -o "${CITY}" --utm_zone 17 --utm_hemisphere N -t 20 -n 100

echo "${CMD[*]}"
${CMD[*]}
