# OMPL_Python_bindings

# Steps to Install OMPL with Python Bindings

1. First, make sure you have all the required library dependencies installed:
```bash
sudo apt update
sudo apt install libboost-all-dev cmake libeigen3-dev
sudo apt install python3-pip python3-pyparsing python3-numpy
pip3 install pygccxml pyplusplus castxml
```

2. **IMPORTANT STEP!**: install the compatible versions of these libraries:
```bash
pip install 'importlib_metadata<4.0'
pip install 'pygccxml==2.2.1'
```

3. Navigate to your `home` (or `root`, if using Docker) directory, and clone the ompl github repository with the **python bindings fix**:

```bash
git clone --depth 1 --branch fix/pybindings-generation-latest https://github.com/ompl/ompl.git
```

4. Go to the `ompl` directory you just cloned:

```bash
cd ompl
```

5. Make a directory where you will build the OMPL library and go to the new directory

```bash
mkdir -p build/Release
cd build/Release
```

6. Run the cmake command with `PYTHON_BINDINGS` enabled and the libboost library specified:

```bash
cmake ../.. -DOMPL_BUILD_PYBINDINGS=ON -DPYTHON_EXECUTABLE=$(which python3) -DBoost_PYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libboost_python38.so
```

7. Next, call the make command to generate the python bindings:

```bash
make -j 4 update_bindings
```

8. Then, compile the OMPL library:

```bash
make -j4
```

9. Finally, install the library and bindings to your system (or container, if in Docker):

```bash
sudo make install
```

10. Confirm OMPL is installed with python bindings:

```bash
python
from ompl import base
```

If no error like this (below) pops up, then congratulations! You have installed OMPL with python bindings!

```bash
    from ompl.util._util import *
ModuleNotFoundError: NO module named 'ompl.util._util'
```
*NOTE: If the error (above) pops up, check out this thread [(here)](https://github.com/ompl/ompl/issues/1110) and see possible fixes. It could be an issue with your PyPy library version, or some other library.*

11. **PS:** If you're using Docker, don't forget to commit the container after the library is installed!

```bash
docker commit <container_id or container_name> <image_id> 
```

