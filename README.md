poco-cmake
==========

CMake scripts and support files for the POCO C++ Libraries and the POCO Open Service Platform (OSP).

These scripts are provided as is and are not (yet?) officially endorsed by Applied Informatics Software Engineering GmbH. They have not been tested with versions of the OSP Library released later than 2008, but are believed to work for newer versions as well.

Author: 
Andreas Stahl andreas.stahl@tu-dresden.de
Research Assistant, Lehrstuhl für Mediengestaltung, Technische Universität Dresden


Usage
-----

Put the contents of the cmake dir in your cmake module and include paths.

To build a single library bundle in your CMakeLists.txt file, instead of declaring a library target (add_library), use the following command

```cmake
poco_add_single_library_bundle(<library_target_name> <bundle_target_name> 
    VERSION <1.0.0> ACTIVATOR_CLASS <activator_class> [FOLDER <ide_folder_name>]
    <src1> [src2...])
```

This creates two targets, one for the (shared) library and one for the bundle that will contain that library as an "activator". Use the library target for linking to common libraries and the bundle target for bundle related purposes, like dependency declarations, packaging of files, etc.

```cmake
poco_finalize_bundle(<bundle_target> [COPY_TO <executable_target | directory>])
```
Wraps up a bundle and optionally copies the product to a (default) "bundles" subdirectory in the directory of an executable target.

```cmake
poco_install_bundle(TARGETS <bundle_target>
	[[BUNDLE | LIBRARY | RUNTIME | ARCHIVE] DESTINATION <dest>] [...]
)
```
This special install command replicates a subset of the standard install command syntax, with one exception: BUNDLE DESTINATION is the directory that the POCO OSP bundle will be installed to. The LIBRARY, RUNTIME and ARCHIVE signatures handle the libraries associated with the bundle and may be omitted if only the bundle is to be used by clients and import libraries will not be distributed with the installation. 

Examples
--------

Prerequisites: 
- POCO OSP: You need to install POCO OSP v2008p2 or later, and the runtime libraries need to be discoverable, i.e. in a standard location (/usr/local/ for linux and OS X or C:/Program Files/Poco on windows). Alternatively you can direct CMake to the correct location by either setting the POCO_DIR Environment variable or passing -DPoco_DIR="path/to/poco/" as a parameter to cmake. 
- CMake: Also required of course is CMake, I recommend a version > 2.8.10.2, at the moment that means the latest nightly build, as there have been some fixes to generator expressions that are needed for the scripts to work.

### Linux / OS X

To build the examples, use cmake on the command line

```bash
cd examples
mkdir out
cd out
cmake .. [-G Xcode | -G Ninja | ... ]
make [xcodebuild | ninja | ... ]
```

or point the cmake gui to the examples folder as source location.

### MS Visual Studio

The examples are not yet up and running on Visual Studio out of the box, but I'm working on it. You can see how far you get by the following method.
Use the browse source button in CMake to point "Where is the source code" to the examples directory. Set the "Where to build the binaries" line to a directory of your choosing, which will set the project's binary directory, with an out-of-source build highly recommended, i.e. point it to a different directory from where the source lies.

Click configure. If this fails, you might need to expand the "Ungrouped Entries" in the cache variables view and set the Poco_ROOT_DIR variable to the location of your POCO installation, if it's in a non-standard place, then hit configure again. If everything checks out okay (no red messages in the lower log-view), push "Generate", which will create a Visual Studio solution in the binary directory. Open this solution. The project explorer shows the different example targets, as well as the standard cmake targets ALL_BUILD and INSTALL. Build the ALL_BUILD target, set bundle-container as startup project, cross your fingers and hit F5 (Debug).


TODOs
-----

- [ ] clean up and document all new target properties.
- [ ] document all new functions.
- [ ] optimize target dependencies to avoid unecessary build steps.
- [ ] more complex examples with interdependencies and extension points.


License
-------

The Boost Software License 1.0

Permission is hereby granted, free of charge, to any person or organization obtaining a copy of the software and accompanying documentation covered by this license (the "Software") to use, reproduce, display, distribute, execute, and transmit the Software, and to prepare derivative works of the Software, and to permit third-parties to whom the Software is furnished to do so, all subject to the following:
The copyright notices in the Software and this entire statement, including the above license grant, this restriction and the following disclaimer, must be included in all copies of the Software, in whole or in part, and all derivative works of the Software, unless such copies or derivative works are solely in the form of machine-executable object code generated by a source language processor.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. IN NO EVENT SHALL THE COPYRIGHT HOLDERS OR ANYONE DISTRIBUTING THE SOFTWARE BE LIABLE FOR ANY DAMAGES OR OTHER LIABILITY, WHETHER IN CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.