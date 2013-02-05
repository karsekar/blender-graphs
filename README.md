Networks in Blender
============================

Produces visualizations of network data.

Usage
-----

To use standard parameters for everything, simply run

```bash
sh draw_network.sh *adjacency_list*
```

where *adjacency_list* is either a file or a string containing a json adjacency
list of the format

```json
[["source_1", "target_1"], ["source_2", "target_2"], ...]
```

The node identifiers can be anything, so long as they are self consistent.
For example,

```bash
sh draw_network.sh "[[1,2],[2,3],[3,4],[4,1],[2,5],[5,4]]"
```

will open blender with a 3D visualization of this network.


Samples
-------

####Visualization produced by above example
![](http://www.patrick-fuller.com/wp-content/uploads/2013/01/trigonal_wheel.png)

####Les Misérables character data (data copied from [this d3 example](http://bl.ocks.org/4062045))
![](http://www.patrick-fuller.com/wp-content/uploads/2013/01/les_miserables.png)

####Binary red-black(-ish) tree
![](http://www.patrick-fuller.com/wp-content/uploads/2013/01/red_black_tree_3d.png)

####Breadth-first and depth-first searches on 2D network (Freestyle render)
![](http://www.patrick-fuller.com/wp-content/uploads/2013/02/queries.png)

Advanced Usage
--------------

For more control, you can break the process into two parts. Running

```bash
# Run this for force-directed layouts, or
python force_directed_layout.py "[[1,2],[2,3],[3,4]]" > network.json
# this for random layouts
python random_layout.py "[[1,2],[2,3],[3,4]]" > network.json
```

produces a `network.json` file containing

```json
{
    "edges": [
        {
            "source": 1, 
            "target": 2
        }, 
        {
            "source": 2, 
            "target": 3
        }, 
        {
            "source": 3, 
            "target": 4
        }
    ], 
    "nodes": {
        "1": {
            "location": [ 9.339, -17.667, -2.138 ]
        }, 
        "2": {
            "location": [ 8.877, -7.235, -10.665 ]
        }, 
        "3": {
            "location": [ 3.765, -0.434, -19.326 ]
        }, 
        "4": {
            "location": [ 0, 0, 0 ]
        }
    }
}
```

If you want to run your own location-generating code, the Blender script will
run on any `network.json` file with this format. Then, run

```bash
blender network.blend -P network_to_blender.py
```

which will open a blender gui with the chosen network.

Splitting gives you access to some additional network-generation parameters.

```bash
python force_directed_layout.py --force-strength 10 --2D "[[1,2],[2,3],[3,4]]"
```

 * `--force-strength` determines the separation between nodes
 * `--2D` confines the network layout to two dimensions


```bash
python random_layout.py --edge-length 15 --separation 3 --density 60 --concentric --2D "[[1,2],[2,3],[3,4]]"
```

 * `--edge-length` is the maximum length of a network edge
 * `--separation` is the minimum distance between any two nodes
 * `--density` attempts to compact the nodes spherically if non-zero
 * `--concentric` places the root node at the center of the network
 * `--2D` confines the network layout to two dimensions

The `network_to_blender.py` script can also be easily edited to change node
shapes, sizes, and colors, or to disable edge arrows (it's a short script!).

