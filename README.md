# d3-voronoi

This module implements [Steven J. Fortune’s algorithm](https://en.wikipedia.org/wiki/Fortune's_algorithm) for computing the [Voronoi diagram](https://en.wikipedia.org/wiki/Voronoi_diagram) or [Delaunay triangulation](https://en.wikipedia.org/wiki/Delaunay_triangulation) of a set of two-dimensional points. This implementation is largely based on [work by Raymond Hill](http://www.raymondhill.net/voronoi/rhill-voronoi.html).

Voronoi diagrams are not only [visually](http://bl.ocks.org/mbostock/4360892) [attractive](http://bl.ocks.org/mbostock/4636377) but practical tools for interaction, such as to increase the target area of points in a scatterplot. See [“Strikeouts on the Rise”](http://www.nytimes.com/interactive/2013/03/29/sports/baseball/Strikeouts-Are-Still-Soaring.html) in *The New York Times* and this [multi-line chart](http://bl.ocks.org/mbostock/8033015) for examples; also see Tovi Grossman’s paper on [bubble cursors](http://www.tovigrossman.com/BubbleCursor) for a related technique. Voronoi diagrams can also be used to [automate label positioning](http://bl.ocks.org/mbostock/6909318), and Delaunay meshes are useful in computing adjacency or grouping of visual elements.

<a href="http://bl.ocks.org/mbostock/6675193"><img src="http://bl.ocks.org/mbostock/raw/6675193/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/4060366"><img src="http://bl.ocks.org/mbostock/raw/4060366/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/4341156"><img src="http://bl.ocks.org/mbostock/raw/4341156/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/4360892"><img src="http://bl.ocks.org/mbostock/raw/4360892/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/7608400"><img src="http://bl.ocks.org/mbostock/raw/7608400/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/4636377"><img src="http://bl.ocks.org/mbostock/raw/4636377/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/1073373"><img src="http://bl.ocks.org/mbostock/raw/1073373/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/8033015"><img src="http://bl.ocks.org/mbostock/raw/8033015/thumbnail.png" width="202"></a>
<a href="http://bl.ocks.org/mbostock/c6966db1fcb0ed2988da"><img src="http://bl.ocks.org/mbostock/raw/c6966db1fcb0ed2988da/thumbnail.png" width="202"></a>

## Installing

If you use NPM, `npm install d3-voronoi`. Otherwise, download the [latest release](https://github.com/d3/d3-voronoi/releases/latest). The released bundle supports AMD, CommonJS, and vanilla environments. Create a custom build using [Rollup](https://github.com/rollup/rollup) or your preferred bundler. You can also load directly from [d3js.org](https://d3js.org):

```html
<script src="https://d3js.org/d3-voronoi.v0.2.min.js"></script>
```

In a vanilla environment, a `d3_voronoi` global is exported. [Try d3-voronoi in your browser.](https://tonicdev.com/npm/d3-voronoi)

## API Reference

<a name="voronoi" href="#voronoi">#</a> d3.<b>voronoi</b>()

Creates a new Voronoi layout with default [*x*-](#voronoi_x) and [*y*-](#voronoi_y)accessors and a null [extent](#voronoi_extent).

<a name="_voronoi" href="#_voronoi">#</a> <i>voronoi</i>(<i>data</i>)

Computes the Voronoi diagram for the specified *data* points. See [voronoi diagrams](#voronoi-diagrams) for details on the returned data structure. Warning: if any points are coincident or have NaN positions, **the behavior of this method is undefined.** Most likely, invalid polygons will be returned! You must filter invalid points and consolidate coincident points before computing.

<a name="voronoi_x" href="#voronoi_x">#</a> <i>voronoi</i>.<b>x</b>([<i>x</i>])

If *x* is specified, sets the *x*-coordinate accessor. If *x* is not specified, returns the current *x*-coordinate accessor, which defaults to:

```js
function x(d) {
  return d[0];
}
```

<a name="voronoi_y" href="#voronoi_y">#</a> <i>voronoi</i>.<b>y</b>([<i>y</i>])

If *y* is specified, sets the *y*-coordinate accessor. If *y* is not specified, returns the current *y*-coordinate accessor, which defaults to:

```js
function y(d) {
  return d[1];
}
```

<a name="voronoi_extent" href="#voronoi_extent">#</a> <i>voronoi</i>.<b>extent</b>([<i>extent</i>])

If *extent* is specified, sets the clip extent of the Voronoi layout to the specified bounds and returns the layout. The *extent* bounds are specified as an array [​[<i>x0</i>, <i>y0</i>], [<i>x1</i>, <i>y1</i>]​], where <i>x0</i> is the left side of the extent, <i>y0</i> is the top, <i>x1</i> is the right and <i>y1</i> is the bottom. If *extent* is not specified, returns the current clip extent which defaults to null. A clip extent is required in conjunction with [*voronoi*.polygons](#voronoi_polygons), as unclipped polygons may have large coordinates which may not render correctly.

<a name="voronoi_size" href="#voronoi_size">#</a> <i>voronoi</i>.<b>size</b>([<i>size</i>])

An alias for [*voronoi*.extent](#voronoi_extent) where the minimum *x* and *y* of the extent are ⟨0,0⟩. Equivalent to:

```js
voronoi.extent([[0, 0], size]);
```

<a name="voronoi_polygons" href="#voronoi_polygons">#</a> <i>voronoi</i>.<b>polygons</b>(<i>data</i>)

Returns an array of polygons, one for each input point in the specified *data* points, corresponding to the cells in the computed Voronoi diagram. This is a convenience method equivalent to:

```js
voronoi(data).polygons(voronoi.extent());
```

See [*diagram*.polygons](#diagram_polygons) for more detail.

<a name="voronoi_triangles" href="#voronoi_triangles">#</a> <i>voronoi</i>.<b>triangles</b>(<i>data</i>)

Returns the Delaunay triangulation of the specified *data* array as an array of triangles. Each triangle is a three-element array of elements from *data*. This is a convenience method equivalent to:

```js
voronoi(data).triangles();
```

See [*diagram*.triangles](#diagram_triangles) for more detail.

<a name="voronoi_links" href="#voronoi_links">#</a> <i>voronoi</i>.<b>links</b>(<i>data</i>)

Returns the Delaunay triangulation of the specified *data* array as an array of links. Each link has the following attributes:

* `source` - the source node, an element in *data*.
* `target` - the target node, an element in *data*.

This is a convenience method equivalent to:

```js
voronoi(data).links();
```

See [*diagram*.links](#diagram_links) for more detail.

### Voronoi Diagrams

<a name="diagram" href="#diagram">#</a> <i>diagram</i>

The computed Voronoi diagram returned by [*voronoi*](#_voronoi) has the following properties:

* `cells` - an array of [cells](#diagram_cell), one per input point.
* `edges` - an array of [edges](#diagram_edge).

<a name="diagram_polygons" href="#diagram_polygons">#</a> <i>diagram</i>.<b>polygons</b>(<i>extent</i>)

Returns an array of polygons, one for each cell in the diagram. Each polygon is represented as an array of points [*x*, *y*] where *x* and *y* are the point coordinates, and a `data` field that refers to the corresponding element in *data*. Polygons are *open* in that they do not contain closing points that duplicate the initial point; a triangle, for example, is an array of three points. Polygons are also counterclockwise, assuming the origin ⟨0,0⟩ is in the top-left corner.

<a name="diagram_triangles" href="#diagram_triangles">#</a> <i>diagram</i>.<b>triangles</b>()

Returns the Delaunay triangulation of the specified *data* array as an array of triangles. Each triangle is a three-element array of elements from *data*.

<a name="diagram_links" href="#diagram_links">#</a> <i>diagram</i>.<b>links</b>()

Returns the Delaunay triangulation of the specified *data* array as an array of links, one for each edge in the mesh. Each link has the following attributes:

* `source` - the source node, an element in *data*.
* `target` - the target node, an element in *data*.

<a name="cell" href="#cell">#</a> <i>cell</i>

Each cell has the following properties:

* `site` - the [site](#site) of the cell’s associated input point.
* `halfedges` - an array of indexes into [*diagram*.edges](#diagram), in counterclockwise order, representing the cell’s polygon.

<a name="site" href="#site">#</a> <i>site</i>

Each site is an array [*x*, *y*] with two additional properties:

* `index` - the site’s index, corresponding to the associated input point.
* `data` - the input data corresponding to this site.

<a name="edge" href="#edge">#</a> <i>edge</i>

Each edge is an array [​[*x0*, *y0*], [*x1*, *y1*]] with two additional properties:

* `left` - the [site](#site) on the left side of the edge
* `right` - the [site](#site) on the right side of the edge
