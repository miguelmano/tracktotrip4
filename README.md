# TrackToTrip
*TrackToTrip* is a library to process GPS tracks.

The main goals are to transform a (gpx) **track into a trip**.

> **track**
> raw representation of a GPS recording. It is not precise, has noise and valuable information is hidden.


> **trip**
> result of one or more processed tracks. Its start and end points have semantic meaning, such as home, work or school. It has less errors and it's compressed, with as little information loss as possible. In short, a trip is an approximation of the true path recorded.

## Installing

You can install TrackToTrip by running the following command:

```
 $ python setup.py install
```

**NOTE:** TrackToTrip requires Microsoft Visual C++ 14.0. It can be found using the [Build Tools for Visual Studio 2022](https://visualstudio.microsoft.com/downloads/?q=build+tools)


**Python 3.x** is required.

## Overview

The starting points are the [Track](../master/tracktotrip/track.py), [Segment](../master/tracktotrip/segment.py) and [Point](../master/tracktotrip/point.py) classes.

### [Track](../master/tracktotrip/track.py)

Can be loaded from a GPX file:

```python
from tracktotrip import Track, Segment, Point

track = Track.from_gpx('file_to_track.gpx')
```

A track can be transformed into a trip with the method ` to_trip `. Transforming a track into a trip executes the following steps:

1. Smooths the segments, using the [kalman filter](../master/tracktotrip/smooth.py)

2. Spatiotemporal segmentation for each segment, using the [DBSCAN algorithm](../master/tracktotrip/spatiotemporal_segmentation.py) to find spatiotemporal clusters

3. Compresses every segment, using [spatiotemporal-aware compression algorithm](../master/tracktotrip/compression.py)

A track is composed by ` Segment `s, and each segment by ` Point `s.

It can be saved to a GPX file:

```python
with open('file.gpx', 'w') as f:
  f.write(track.to_gpx())
```

### [Segment](../master/tracktotrip/segment.py)

A Segment holds the points, the transportation modes used, and the start and end semantic locations.

### [Point](../master/tracktotrip/point.py)

A Point holds the position and time. Currently the library doesn't support elevation.


## License

[MIT license](../master/LICENSE)
