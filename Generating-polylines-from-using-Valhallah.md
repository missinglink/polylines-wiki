Mapzen created an opensource routing engine called [Valhalla](https://github.com/valhalla).
An [export script](https://github.com/valhalla/tools/pull/68) was added to the valhalla tools repo which enables exporting the routing graph as polylines.
The polylines can then be imported using the scripts in this repo.

This document outlines the process of installing the tools, fetching the data and creating the extracts.

## Building valhalla
Use the [installation instructions](https://github.com/valhalla/tools#building-and-running-valhalla) on the valhalla repo to compile the tools.

The server and demo are optional, you probably won't be running an instance of the routing engine so you may elect to skip these parts.

If you are rebuilding after updating the software you may need to rebuild all the tools again, as a change in one repo may be required in another.

The build itself can take some time, sit back and grab a coffee.

### Gotchas

On more recent versions of Ubuntu you might find some of the `apt` versions have changed. eg. if you get a message such as:

```
E: Unable to locate package libboost1.54-all-dev
```

You can substitute it for a newer version such as: `libboost1.58-all-dev`.

```
$ apt-cache search libboost1
libboost1.58-dbg - Boost C++ Libraries with debug symbols
libboost1.58-dev - Boost C++ Libraries development files
libboost1.58-doc - Boost.org libraries documentation
libboost1.58-tools-dev - Boost C++ Libraries development tools
libboost1.58-all-dev - Boost C++ Libraries development files (ALL)
```
