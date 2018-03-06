Mapzen created an opensource routing engine called [Valhalla](https://github.com/valhalla).

An [export script](https://github.com/valhalla/valhalla/blob/master/src/valhalla_export_edges.cc) was added to the valhalla tools repo which enables exporting the routing graph as polylines.

The polylines can then be imported using the scripts in this repo.

This document outlines the process of installing the tools, fetching the data and creating the extracts.

## Building valhalla
Use the [installation instructions](https://github.com/valhalla/valhalla#documentation) on the valhalla repo to compile the tools.

The server and demo are optional, you probably won't be running an instance of the routing engine so you may elect to skip these parts.

If you are rebuilding after updating the software you may need to rebuild all the tools again, as a change in one repo may be required in another.

The build itself can take some time, sit back and grab a coffee.

### Confirm

Once the tools are installed you should be able to find them on your `PATH`:

```sh
which valhalla_export_edges
# this should return the following: /usr/local/bin/valhalla_export_edges
```

and to check if they are useable:

```sh
valhalla_build_tiles --help
```

### Gotchas

If you have an error regarding missing shared object files, you will need to run the following command as root or with sudo priviliges:

```sh
ldconfig
```

## Configuration

Build a config file according to the [documentation](https://github.com/valhalla/valhalla#running):

```sh
mkdir -p valhalla_tiles
valhalla_build_config --mjolnir-tile-dir ${PWD}/valhalla_tiles --mjolnir-tile-extract ${PWD}/valhalla_tiles.tar --mjolnir-timezone ${PWD}/valhalla_tiles/timezones.sqlite --mjolnir-admin ${PWD}/valhalla_tiles/admins.sqlite > valhalla.json
```

## Generating tiles

Tiles can be pre-generated and hosted or cut on demand. If you already have tiles you can skip this section.

The install script above includes a script which generated tiles for `switzerland` and `liechtenstein`:

```
valhalla_build_tiles -c valhalla.json switzerland-latest.osm.pbf liechtenstein-latest.osm.pbf
```

## Cutting polyline extracts

Once you have created/downloaded tiles for valhalla you can use the `valhalla_export_edges` script to export the road graph in polyline format.

```
$ valhalla_export_edges --help

 Usage: valhalla_export_edges [options]

valhalla_export_edges is a simple command line test tool which dumps information about each graph edge. 

:
  -h [ --help ]         Print this help message.
  -v [ --version ]      Print the version of this software.
  -c [ --column ] arg   What separator to use between columns [default=\0].
  -r [ --row ] arg      What separator to use between row [default=\n].
  --config arg          Valhalla configuration file
```

An extract can be produced with a command such as this, make sure you redirect the `stdout` stream somewhere.

```
$ valhalla_export_edges --config valhalla.json > my_extract.polyline
2016/08/16 09:27:15.865953 [INFO] Enumerating edges...
...
```

### How long does it take?

It's fast!, it takes about 7 minutes to cut the whole planet, much less for fewer tiles.