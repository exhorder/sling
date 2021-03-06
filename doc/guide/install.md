# SLING Installation and Building

## Trying out the parser

If you just want to try out the parser on a pre-trained model, you can install
the wheel with pip and download a pre-trained parser model. On a Linux machine
with Python 2.7 you can install a pre-built wheel:

```
sudo pip install http://www.jbox.dk/sling/sling-2.0.0-cp27-none-linux_x86_64.whl
```
and download the pre-trained model:
```
wget http://www.jbox.dk/sling/caspar.flow
```
You can then use the parser in Python:
```
import sling

parser = sling.Parser("caspar.flow")

text = raw_input("text: ")
doc = parser.parse(text)
print doc.frame.data(pretty=True)
for m in doc.mentions:
  print "mention", doc.phrase(m.begin, m.end)
```

## Installation

If you want to train a parser or use SLING for C++ development, you need to
download the source code and build it.

First, clone the GitHub repository.

```shell
git clone https://github.com/google/sling.git
cd sling
```

SLING uses [Bazel](https://bazel.build/) as the build system, so you need to
[install Bazel](https://docs.bazel.build/versions/master/install.html) in order
to build the SLING parser.

```shell
sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python
wget -P /tmp https://github.com/bazelbuild/bazel/releases/download/0.13.0/bazel-0.13.0-installer-linux-x86_64.sh
chmod +x /tmp/bazel-0.13.0-installer-linux-x86_64.sh
sudo /tmp/bazel-0.13.0-installer-linux-x86_64.sh
```

The parser trainer uses Python v2.7 and PyTorch for training, so they need to be
installed.

```shell
sudo pip install http://download.pytorch.org/whl/cpu/torch-0.3.1-cp27-cp27mu-linux_x86_64.whl
```

## Building

Operating system: Linux<br>
Languages: C++, Python 2.7, assembler<br>
CPU: Intel x64 or compatible<br>
Build system: Bazel<br>

You can use the `buildall.sh` script to build all the source code:

```shell
tools/buildall.sh
```

You then need to link the sling Python module directly to the Python source
directory to use it in "developer mode":

```shell
sudo ln -s $(realpath python) /usr/lib/python2.7/dist-packages/sling
```

**NOTE:**
* In case you are using an older version of GCC (< v5), you may want to comment
  out [this cxxopt](https://github.com/google/sling/blob/f8f0fbd1a18596ccfe6dbfba262a17afd36e2b5f/.bazelrc#L8) in .bazelrc.
* We currently do not support OSX, but you can check out
  [issue #189](https://github.com/google/sling/issues/189) for help on building
  on OSX.
* Similarly, we do not support Windows, but you can check out
  [issue #296](https://github.com/google/sling/issues/296) for help on
  building SLING on Windows Subsystem for Linux (WSL).
