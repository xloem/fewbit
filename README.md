# few-bit-backward

## Assembly

Preliminary step depends on one's PyTorch distribution and availiable tooling.
Building of native components requires CMake and a build system like Make or Ninja.
Next, if PyTorch is installed system-wide the the following step is not neccessary.
Otherwise, one likely should add search path for CMake modules to environment variables as follows.

```shell
export CMAKE_PREFIX_PATH="$(python -c 'import torch.utils; print(torch.utils.cmake_prefix_path)')"
```

The next step is useful in development environment.
It just builds PyTorch operator library in source tree (option `--inplace`) with forced CUDA support (option `--cuda`).
By default no CUDA support are forced.

```shell
python setup.py build_ext --inplace --cuda
```

With options similar to the previous step, one can build wheel binary distribution of the package.

```shell
python setup.py bdist_wheel --inplace --cuda
```

# Development Environment with Docker

In order to develop on different platforms we uses custom docker image for non-priviledge user based on Nvidia CUDA image.
Image contains pre-built native extention and it is parametrized by user name and user ID in a host system.
The latter is crucial thing in binding host volumes.

```shell
docker build -t few-bit-backward --build-arg UID=$(id -u) .
docker run --rm -ti -e TERM=$TERM few-bit-backward
```
