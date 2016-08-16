Mapzen created an opensource routing engine called [Valhalla](https://github.com/valhalla).
An [export script](https://github.com/valhalla/tools/pull/68) was added to the valhalla tools repo which enables exporting the routing graph as polylines.
The polylines can then be imported using the scripts in this repo.

This document outlines the process of installing the tools, fetching the data and creating the extracts.

## Building valhalla
Use the [installation instructions](https://github.com/valhalla/tools#building-and-running-valhalla) on the valhalla repo to compile the tools.

The server and demo are optional, you probably won't be running an instance of the routing engine so you may elect to skip these parts.

If you are rebuilding after updating the software you may need to rebuild all the tools again, as a change in one repo may be required in another.

The build itself can take some time, sit back and grab a coffee.


