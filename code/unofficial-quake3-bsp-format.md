# Unofficial Quake 3 BSP Format #
Author:     Ben "Digiben" Humphrey  


## Introduction ##

This document was created as an aid to the **Quake3 BSP** tutorial series 
featured on www.GameTutorials.com.      The information is what I have 
found, and it's possibly that it's incorrect or just blatantly wrong.  I 
suggest you use this as a reference and a guide, not the end all file 
format doc for the Quake3 .bsp file format.  With that out of the way, 
let's load some sweet levels!  

The Quake3 level format, .bsp, stores most of the information about the 
level.  There are other files such as .shader, .arena and .aas, which 
store bot and texture shader information.  The .bsp file is stored in what 
is called a IBSP format.  That means that the length and offsets of 
different sections in the file are stored in what's know as **lumps**.  The 
older version of Quake use this same lump format, but different 
information is stored in each version of Quake.  If you can read in Quake 
3 levels, it's not a lot of changes to write a Quake 2 level loader. 

If you don't know what BSP stands for yet, it means **Binary Space 
Partition(ing)**.  You would create a BSP tree.  That means that there is a 
parent node, and at most, 2 children attached to each parent.  These 
children are called the **front** and **back** children.  I won't attempt to teach 
you how to create or manage a BSP tree here, but there is a BSP FAQ that 
SGI put out floating around the internet somewhere that has a ton of 
information.  Better yet, I suggest you take the BSP class at 
www.GameInstitute.com.  I personally took this class and was quite 
satisfied.  It teaches all you need to know about BSP trees. 



## Lumps ##

Like we mentioned before, lumps hold the length in bytes and offset into 
the file for a given section.  Below is an enum eLumps that holds all the 
lumps and their order in the file: 

	enum eLumps
	{
	   kEntities = 0,     // Stores player/object positions, etc...
	   kTextures,         // Stores texture information
	   kPlanes,           // Stores the splitting planes
	   kNodes,            // Stores the BSP nodes
	   kLeafs,            // Stores the leafs of the nodes
	   kLeafFaces,        // Stores the leaf's indices into the faces
	   kLeafBrushes,      // Stores the leaf's indices into the brushes
	   kModels,           // Stores the info of world models
	   kBrushes,          // Stores the brushes info (for collision)
	   kBrushSides,       // Stores the brush surfaces info
	   kVertices,         // Stores the level vertices
	   kMeshVerts,        // Stores the model vertices offsets
	   kShaders,          // Stores the shader files (blending, anims..)
	   kFaces,            // Stores the faces for the level
	   kLightmaps,        // Stores the lightmaps for the level
	   kLightVolumes,     // Stores extra world lighting information
	   kVisData,          // Stores PVS and cluster info (visibility)
	   kMaxLumps          // A constant to store the number of lumps
	};

Each on of these sections has a offset and a size in bytes that need to be 
read in.  In the next sections we will examine the structures needed to 
read in each lump. 

Here is a lump structure.  The offset is the position into the file that 
is the starting point of the current section.  The length is the number of 
bytes that this lump stores. 


	struct tBSPLump
	{
	   int offset;
	   int length;
	}; 


Let's give an example of reading in the vertices (kVertices) for the 
level.  Once the lumps are read in, to find the number of vertices in the 
level we do this: 

	numOfVerts = lumps[kVertices].length / sizeof(tBSPVertex);

We index the **lumps[]** array with the kVertices constant, then divide that 
lumps length by the size of the tBSPVertex structure in bytes, which we 
will define later on.  It just so happens it's 44 bytes.  If the length is 
3388, then 3388 / 44 = 77.  We now know there is 77 vertices in the .bsp 
file.  We then need to position the file pointer to the lump's offset, and 
start reading in 77 tBSPVertex structures into our dynamically allocated 
vertex array.  I use fread() and fseek() for the file manipulation.  This 
is of course, ONLY if you are not reading from the .zip file.  I am 
strictly speaking of loading the .bsp file unzipped. 

Now that we understand the basics of lumps, let's move on to the header 
structure, along with the rest of the structures for each lump read in.

## BSP Header ##

The very first thing that needs to be read in for the .bsp file is the 
header.  The header contains a 4 character ID, then an integer that holds 
the version. 

	struct tBSPHeader
	{
	   char strID[4];     // This should always be 'IBSP'
	   int version;       // This should be 0x2e for Quake 3 files
	}; 

## Vertices ##

This structure stores the vertex information.  There is a position, 
texture and lightmap coordinates, the vertex normal and color.  To 
calculate the number of vertices in the lump you divide the length of the 
lump by the sizeof(tBSPVertex). 

	struct tBSPVertex
	{
	   float vPosition[3];      // (x, y, z) position. 
	   float vTextureCoord[2];  // (u, v) texture coordinate
	   float vLightmapCoord[2]; // (u, v) lightmap coordinate
	   float vNormal[3];        // (x, y, z) normal vector
	   byte color[4];           // Color color for the vertex 
	};

## Faces ##

This structure holds the face information for each polygon of the level.  
It mostly holds indices into all the vertex and texture arrays.  To 
calculate the number of faces in the lump you divide the length of the 
lump by the sizeof(tBSPFace). 

	struct tBSPFace
	{
	   int textureID;        // The index into the texture array 
	   int effect;           // The index for the effects (or -1 = n/a) 
	   int type;             // 1=polygon, 2=patch, 3=mesh, 4=billboard 
	   int vertexIndex;      // The index into this face's first vertex 
	   int numOfVerts;       // The number of vertices for this face 
	   int meshVertIndex;    // The index into the first meshvertex 
	   int numMeshVerts;     // The number of mesh vertices 
	   int lightmapID;       // The texture index for the lightmap 
	   int lMapCorner[2];    // The face's lightmap corner in the image 
	   int lMapSize[2];      // The size of the lightmap section 
	   float lMapPos[3];     // The 3D origin of lightmap. 
	   float lMapBitsets[2][3]; // The 3D space for s and t unit vectors. 
	   float vNormal[3];     // The face normal. 
	   int size[2];          // The bezier patch dimensions. 
	};

If the face **type** is 1 (normal polygons), the vertexIndex and numOfVerts 
can be used to index into the vertex array to render triangle fans.

If the face **type** is 2 (bezier path), the vertexIndex and numOfVerts act 
as a 2D grid of control points, where the grid dimensions are described by 
the size[2] array.  You can render the bezier patches with just the 
vertices and not fill in the curve information, but it looks horrible and 
blocky.   

The point of the curved surfaces are to be able to create a more defined 
surface, depending on the specs of the computer that is running that 
application.  Some computers with horrible speed and video cards would 
make the smallest amount of polygons from the curve, where as the fast 
computers using Geforce cards could use the highest amount of polygons to 
form a perfect curve.

If the face **type** is 3 (mesh vertices), the vertexIndex and numOfVerts 
also work the same as if the type is 1

If the face **type** is 4, the vertexIndex is the position of the billboard.  
The billboards are used for light effects such as flares, etc... 



## Textures ##

The texture structure stores the name of the texture, along with some 
surface information which are associated with the brush, brush sides and 
faces.  To calculate the number of textures in the lump you divide it's  
length by the sizeof(tBSPTexture). 

	struct tBSPTexture
	{
	   char strName[64];   // The name of the texture w/o the extension 
	   int flags;          // The surface flags (unknown) 
	   int contents;       // The content flags (unknown)
	};

## Lightmaps ##

Unlike the textures, the lightmaps are stored in the .bsp file as 128x128 
RGB images.  Many faces share the same lightmap with their own section of 
it stored in the lightmap UV coordinates.  Once you read in the lightmaps, 
you will want to create textures from them.  To calculate the number of 
lightmaps  in the lump you divide the length of the lump by the 
sizeof(tBSPLightmap). 

	struct tBSPLightmap
	{
	   byte imageBits[128][128][3];   // The RGB data in a 128x128 image
	};

## Nodes ##

The node structure holds the nodes that make up the BSP tree.  The BSP is 
not used for rendering so much in Quake3, but for collision detection.  
The node holds the splitter plane index, the front and back index, along 
with the bounding box for the node.  If the front or back indices are 
negative, then it's an index into the leafs array.  Since negative numbers 
can't constitute an array index, you need to use the ~ operator or -(index 
+ 1) to find the correct index.  This is because 0 is the starting index. 
To calculate the number of nodes in the lump you divide the length of the 
lump by the sizeof(tBSPNode). 

	struct tBSPNode
	{
	   int plane;      // The index into the planes array 
	   int front;      // The child index for the front node 
	   int back;       // The child index for the back node 
	   int mins[3];    // The bounding box min position. 
	   int maxs[3];    // The bounding box max position. 
	}; 

## Leafs ##

The leafs, like the faces, are a very important part of the BSP 
information.  They store the  visibility cluster, the area portal, the 
leaf bounding box, the index into the faces, the number of leaf faces, the 
index into the brushes for collision, and finally,  the number of leaf 
brushes.  To calculate the number of leafs in the lump you divide the 
length of the lump by the sizeof(tBSPLeaf). 

	struct tBSPLeaf
	{
	   int cluster;           // The visibility cluster 
	   int area;              // The area portal 
	   int mins[3];           // The bounding box min position 
	   int maxs[3];           // The bounding box max position 
	   int leafface;          // The first index into the face array 
	   int numOfLeafFaces;    // The number of faces for this leaf 
	   int leafBrush;         // The first index for into the brushes 
	   int numOfLeafBrushes;  // The number of brushes for this leaf 
	}; 

## Leaf Faces ##

The leaf faces are used to index into tBSPFaces array.  You might at first 
think this is strange to have the tBSPLeaf structure have an index into 
the pLeafFaces array, which in turn is just an index into the tBSPFaces 
array.  This is because it's set up to start with a starting point 
(leafface) and a count to go from there for each face (numOfLeafFaces).  
The faces array is not contiguous (in a row) according to each leaf.  That 
is where the leaf faces array comes into play.  It's kinda like the same 
concept of model loaders where they store the vertices and then have faces 
that store the indices into the vertex array for that face.  To calculate 
the number of leaf faces in the lump you divide the length of the lump by 
the sizeof(int).  

	int *pLeafFaces;           // The index into the face array 

## Planes ##

The plane structure stores the normal to the plane and it's distance to 
the origin.  We use this as the splitter plane for the BSP tree.  When 
rendering or testing collision, we can test the camera position against 
the planes to see which plane we are in front of.  To calculate the number 
of planes in the lump you divide the length of the lump by the 
sizeof(tBSPPlane). 

	struct tBSPPlane
	{
	   float vNormal[3];     // Plane normal. 
	   float d;              // The plane distance from origin 
	};

## Visibility Data ##

The visibility information is comprised of a bunch of bitsets that store a 
bit for every cluster.  This is because the information is so massive that 
this way makes it faster to access and a smaller memory footprint.  There 
is only one instance of this structure, but you calculate how much needs 
to be read in bytes by either: numOfClusters * bytesPerCluster, or minute 
the size of 2 integers from this lumps length.  The pBitsets is then 
dynamically allocated and stores the calculate bytes.  This is probably 
one of the most confusing parts about the .bsp file format, the visibilty. 
I will try and explain the important parts of it and give some code. 

	struct tBSPVisData
	{
	   int numOfClusters;   // The number of clusters
	   int bytesPerCluster; // Bytes (8 bits) in the cluster's bitset
	   byte *pBitsets;      // Array of bytes holding the cluster vis.
	}; 

To demonstrate what a cluster is and what we need to do with it, let's 
start with a simple example.  When rendering, first we want to find which 
leaf we are in.  Once again, a leaf is an end node of the BSP tree that 
holds a bunch of information about the faces, brushes and the cluster it's 
in.  Once that leaf is founding by checking the camera position against 
all of the planes, we then want to go through all of the leafs and check 
if their cluster is visible from the current cluster we are in.  If it is, 
that means that we need to check if that leaf's bounding box is inside of 
our frustum before we draw it.   

Say we have cluster A, B and C.  Each cluster is stored as a bit in 
bitset.  A bitset is just a huge list of binary numbers next to each 
other.  Each cluster has their own list of bits that store a 1 or a 0 to 
tell if the cluster in that bit is visible (1) or not visible (0).  Since 
there is most likely more than 32 clusters in a level, you can't just use 
an integer (32-bits) to store the bits for all the clusters.  This is why 
there are many bytes assigned to each cluster.  So, here is how it works: 

With cluster A, B and C, here is how they would be represented in binary 
(a bitset): 

	ABC
	000 

Each 0 represents a slot that is assigned to a cluster.  Let's assume 
that: 

	- Cluster A can see cluster B and not C
	- Cluster B can see cluster A and C
	- Cluster C can see cluster B and not A 

Below is a representation of each one of their bitsets: 

	- A     110
	- B     111
	- C     011 

Does that make sense?  Notice for A there is a 1 in the first slot which 
means it can see itself, and also in the second slot which means it can 
see cluster B.  The last slot is a 0, which tells us that cluster A can 
not see what's in cluster C because some walls or whatever are blocking 
it.  This is where the good spatial partition speed comes in.  If you are 
in the very bottom corner of the level in a small little room, you just 
cut out probably 95% percent of the polygons that need to be rendered 
because you can only most likely see the cluster that is right outside of 
that room. 

To test if a cluster is visible from another cluster, there is obviously 
going to have to be some bit-shifting and other binary math involved.  The 
basic algorithm to test a cluster against another cluster is as follows: 

	int visible = pBitsets[currentCluster*bytesPerCluster + ( testCluster/8 )] & (1 << (testCluster & 7))  

If the result of visible isn't 0, then the testCluster can be seen from 
the currentCluster.  We divide and % (mod) by 8 because we are using bytes 
which are 8 bits.  Basically, the first part is indexing into the array of 
clusters to find the correct bitset, then we do a binary & (and) with the 
cluster we are testing to get the result. 

Here is some basic code to do a cluster to cluster test: 

	inline int IsClusterVisible(tBSPVisData *pPVS, int current, int test)
	{
	   if(!pPVS->pBitsets || current < 0) return 1;
	
	   byte visSet = pPVS->pBitsets[(current*pPVS->bytesPerCluster) + 
	(test/8)];
	   int result = visSet & (1 << (test & 7));
	
	   return ( result );
	}

## Entities ##

The entity lump just stores a huge string with each line delimited by 
'\n'.  Some types of things stored in this string is the deathmatch 
positions for the players in the beginning, weapon positions, sky box 
information, light positions, etc...  I suggest you save it off to a file 
so that you can get a good idea on how to parse it.  Be careful when 
writing a parser, things aren't always saved in the same order.  Player 
positions usually store the 3D position on the map, along with the 
orientation described by a rotation angle.  To find the length of the 
entity string to read in, just divide the entity lump's length by 
sizeof(char). 

	char *strEntities;      // This stores a huge string of all the entities in the level 

## Brushes ##

The brushes store information about a convex volume, which are defined by 
the brush sides.  Brushes are used for collision detection.  This allows 
the level editor to decide what is collidable and what can be walked 
through, such as trees, bushes or certain corners. 

	struct tBSPBrush 
	{
	   int brushSide;           // The starting brush side for the brush 
	   int numOfBrushSides;     // Number of brush sides for the brush
	   int textureID;           // The texture index for the brush
	}; 

## Leaf Brushes ##

Like the leaf faces, leaf brushes are used to index into the tBSPBrush 
array.  Once again, brushes are used for collision detection.  To 
calculate the number of leaf brushes in the lump you divide the length of 
the lump by the sizeof(int).  

	int *pLeafBrushes;           // The index into the brush array 

## Brush Sides ##

The brush sides lump stores information about the brush bounding surface.  
To calculate the number of brush sides, just divide the lumps length by 
sizeof(tBSPBrushSides). 

	struct tBSPBrushSide 
	{
	   int plane;              // The plane index
	   int textureID;          // The texture index
	}; 

## Models ##

The model structure stores the face and brush information, along with the 
bounding box of the object.  These objects can be movable such as doors, 
platforms, etc...  To calculate the number of models in this lump, just 
divide the length of the lump by sizeof(tBSPModel). 

	struct tBSPModel 
	{
	   float min[3];           // The min position for the bounding box
	   float max[3];           // The max position for the bounding box. 
	   int faceIndex;          // The first face index in the model 
	   int numOfFaces;         // The number of faces in the model 
	   int brushIndex;         // The first brush index in the model 
	   int numOfBrushes;       // The number brushes for the model
	}; 

## Mesh Vertices ##

This stores a list of vertex offsets that are used to create a triangle 
mesh.  To calculate the number of mesh vertices in the lump you divide the 
length of the lump by the sizeof(int).  

	int *pMeshVerts;           // The vertex offsets for a mesh 

## Shaders ##

The shader structure basically gives you the file name, for a *.shader 
file.  The .shader file then stores all of the information about blending, 
animating and such.  The shader files can be found usually in the scripts\ 
folder, assuming that there are any textures in the level that use shaders 
of course.  To calculate the number of shaders for this lump, just divide 
the length of the lump by sizeof(tBSPShader). 

	struct tBSPShader
	{
	   char strName[64];     // The name of the shader file 
	   int brushIndex;       // The brush index for this shader 
	   int unknown;          // This is 99% of the time 5
	}; 

## Light Volumes ##

Not everything in Quake3 is lit by lightmaps.  There are other lights that 
have their own special properties.  I believe you can get the rest of the 
light information from the entities lump.  To calculate the number of 
lights in the lump, just divide the length of the lump by 
sizeof(tBSPLights).

	struct tBSPLights
	{
	   ubyte ambient[3];     // This is the ambient color in RGB
	   ubyte directional[3]; // This is the directional color in RGB
	   ubyte direction[2];   // The direction of the light: [phi,theta] 
	}; 

The light data makes up a 3-Dimensional grid with dimensions of: 

	x = floor(models[0].max[0] / 64)  - ceil(models[0].min[0] / 64)  + 1
	y = floor(models[0].max[1] / 64)  - ceil(models[0].min[1] / 64)  + 1
	z = floor(models[0].max[2] / 128) - ceil(models[0].min[2] / 128) + 1  

## Conclusion ##

This is an on going project to add to this file, so if you have any 
pointers, corrections, or additions, let me know and I will post them.  
Once again, this was used as a tag along with the BSP tutorial series on 
http://www.gametutorials.com/.  I would like to thanks these people for 
their HUGE help on creating this document and understanding the .bsp file 
format: 

Kekoa Proudfoot - kekoa@graphics.stanford.edu 

Bart Sekura - bsekura@poland.com

Ignacio Castano - titan@talika.fie.us.es

Emmanuel Weber - weberemmanuel@hotmail.com

Ben “DigiBen” Humphrey
Game Programmer
DigiBen@GameTutorials.com
Co-Web Host of http://www.gametutorials.com/

The Quake3 file format is owned by ID Software. This tutorial is being 
used as a teaching tool to help understand level loading and advanced 3D
topics. All right reserved. 

©2000-2004 GameTutorials, LLC.  All rights are reserved.
