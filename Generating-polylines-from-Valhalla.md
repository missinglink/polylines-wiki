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

```
peter: .../pelias/polylines (master)$ val
valhalla_benchmark_adjacency_list  valhalla_build_timezones           valhalla_run_isochrone
valhalla_benchmark_admins          valhalla_build_transit             valhalla_run_map_match
valhalla_benchmark_loki            valhalla_elevation_service         valhalla_run_matrix
valhalla_benchmark_skadi           valhalla_export_edges              valhalla_run_route
valhalla_build_admins              valhalla_loki_worker               valhalla_skadi_worker
valhalla_build_connectivity        valhalla_map_match_service         valhalla_thor_worker
valhalla_build_elevation           valhalla_meili_worker              valhalla_tyr_worker
valhalla_build_speeds              valhalla_odin_worker               valhalla_ways_to_edges
valhalla_build_statistics          valhalla_query_transit             validlocale
valhalla_build_tiles               valhalla_route_service 
```

## Configuration

Most valhalla scripts reference the config file `conf/valhalla.json` which can be customized to change the locations of directories on disk. A [default config](https://github.com/valhalla/conf) is used if a custom config is not provided.

If you are downloading pre-generated tiles then you will need to extract them in to your `tile_dir`.

```
peter: .../www/valhalla$ grep tile_dir conf/valhalla.json
    "tile_dir": "/data/valhalla",
```

## Generating tiles

Tiles can be pre-generated and hosted or cut on demand. If you already have tiles you can skip this section.

The install script above includes a script which generated tiles for `switzerland` and `liechtenstein`:

```
LD_LIBRARY_PATH=/usr/lib:/usr/local/lib valhalla_build_tiles -c conf/valhalla.json switzerland-latest.osm.pbf liechtenstein-latest.osm.pbf
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
$ valhalla_export_edges --config conf/valhalla.json > my_extract.polyline
2016/08/16 09:27:15.865953 [INFO] Enumerating edges...
...
```

### Gotchas

If you have an error regarding missing shared object files, you will need to set your `LD_LIBRARY_PATH` env var, see above.

```
$ valhalla_export_edges --config conf/valhalla.json
valhalla_export_edges: error while loading shared libraries: libvalhalla_baldr.so.0: cannot open shared object file: No such file or directory
```

### How long does it take?

It's fast!, it takes about 7 minutes to cut the whole planet, much less for fewer tiles.