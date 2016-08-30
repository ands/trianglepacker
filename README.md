# Triangle Packer
trianglepacker.h is a C/C++ single-file library that packs triangles from a 3D mesh into a 2D map with some specified map size, border and spacing between triangles.
It uses a fast greedy algorithm.
The output is not optimal!

I implemented this to see how it performs in a light mapping context.
Unfortunately, there were often visible seams between adjacent triangles.
Neighbours in the mesh are located in completely different places in the map and thus are not interpolated correctly.
Even slight differences of their edge intensities were very noticeable.
Another disadvantage is, that all mesh vertices will be unique and can not be reused.
I can imagine other use cases for this code, but please tell me yours too ;).

To paste the implementation into your project, insert the following lines:
```
#define TRIANGLEPACKER_IMPLEMENTATION
#include "trianglepacker.h"
```

![Triangle Packer Example Output](https://github.com/ands/trianglepacker/raw/master/example_images/city_packed_border_0_spacing_2.png)

# Example usage
The following example packs all triangles into a specified map resolution and returns the determined 3D->2D scaling factor.
```
float scale3Dto2D;
if (!tpPackIntoRect(
	// mesh (consecutive triangle positions):
	(const float*)mesh->positions, mesh->vertexCount,
	// map (pixel-space parameters):
	map->width, map->height, map->border, map->triangleSpacing,
	// output (a [0..1]x[0..1] uv coordinate for each input vertex):
	mesh->uvs,
	// output (3D units to 2D pixel space scaling factor):
	&scale3Dto2D))
{
	fprintf(stderr, "Could not find a scaling factor to fit all triangles into the map!\n");
	return;
}
```
