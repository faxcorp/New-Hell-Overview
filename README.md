# NewHell 
New Hell - UE5 Environment & Kitbash Toolset 

New Hell is an Unreal Engine 5 Project set in a near future in post-apocalyptic diesel-punk wasteland (satanic), that utilizes many modular pieces and Blueprint tools to construct all assets using splines and instancing. 
The pack was made with kitbashing in mind so all modular pieces uses Nanite and can be scaled up by orders of magnitude. Dynamic materials will scale the textures on the materials accordingly.
Materials are procedural and are using curvature/ambient occlusion/cavity information from vertex colors of the meshes to add details like rust/erosion/cracks. So all materials can be assigned to any of the meshes.

Important Notes: 
The Cliffs PCG graph is heavy and should be disabled when working on the map to avoid frustration. It gets info from buildings and roads and was made with intention to be a post-process generation that doesn't interfere with the layout of the environment.

StaticMesh assets has curvature, ambient occlusion and cavity information baked into their vertex colors. This allows for erosion, color effects without creating separate textures for this many modular pieces. It works in favor when changing assets material, for example, from brick material to metal material, the details like rust or cracks will be drawn procedurally using information in the vertex colors of the StaticMesh.

BP_Building
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/de654ddf-c489-4e72-9c10-bd8713115fd1)

This class is for assembling buildings using other BP Tools like BP_NH_BuildingWall, BP_NH_RadialInstancer, etc.
To create a new building create a Child Actor Class of BP_NH_Building
And then add ChildActorComponents like BP_NH_BuildingWall or BP_NH_RadialInstancer. If it needs a spline, the ChildActorComponent must be attached to the spline and BP_NH_Building class will handle constructing it

BP_NH_BuildingWall
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/5d329821-167a-4de0-a6e2-36fd7dc29b07)

For building walls from a spline, with options to add corners (can align to angle), change spacing, height etc.
The position of doors and windows can be controlled with volumes like BP_NH_DoorWindow_ChildDoor and BP_NH_DoorWindow_ChildWindow. Pieces in these volumes will be switched to meshes from WallsDoors or WallsWindows StaticMesh Array variables. Optionally Actors can be spawned for example opening doors or shattering glasses using DoorActorClass or WindowActorClass Class variables (ChildActorComponents will be added using that class)

BP_NH_DoorWindow_ChildCustomPiece
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/58a4f8e6-0d83-43f4-83c7-fc3cdd256cd2)

This volume can be used to add custom pieces inside it or leave it blank to remove pieces

BP_NH_ArrayInstancer
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/82c9255e-5b36-4f7f-b4ff-2b362c376db1)

Similar to Array modifier in Blender to instancing pieces and offsetting them by a certain amount (for example stairs)

BP_NH_CablePoles
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/6a4477c0-0f53-4661-a239-39f1f0acad98)

Layes out electricity poles with connecting cables using a spline

BP_NH_CablesSupportActor
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/1c529790-f9aa-49bc-af40-0a0a063f0b8f)

Tool for adding a huge pole with cables attached to a spline (external or internal Splines)

BP_NH_CliffSpline
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/d4832d8e-42a4-4923-ac53-22b119b9ec62)

For making cliffs like walls using Spline

BP_NH_Elevator
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/41354ccd-026a-44d8-b011-be5bad4b401c)

Dynamic elevator that can lift or lower players. Uses moving ramp and overlap volumes for triggering

BP_NH_HelixRamp
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/a0d1f3f6-f93a-4302-8183-c3b0585d0442)

A circular ramp that acts like stairs for a player, uses BP_SidewalkActor to create a metal ramp with specified height, radius, and start/end times

BP_NH_SplineMeshActor
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/ec85e9b4-8d66-4e11-9a69-754ad9851070)

Tool to create smooth SplineMeshes using a spline like pipe or road. Can provide distribution meshes like junctions for pipes. Parameters for making generated meshes only draw in Virtual Textures, for example roads.
For optimization there is OptimisationDot float variable that specifies how aggressively to add straight StaticMesh instead of creating SplineMesh. With option to exclusively layout only HISM instances using PlaceOnlyHISMs? bool

BP_NH_Stair
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/fd3f3f8a-10a1-4825-a63f-72e9738e68aa)

Generates a metal staircase with specified Height, Length and width (Scale Y). Optionally can add boolean mesh to make holes for BP_SidewalkActor which works for floors generated by BP_NH_Building or BP_NH_BuildingWall

BP_NH_HydraulicTracedLeg
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/46615b65-a003-4611-ad81-d1cd01ab2546)

Dynamically generated support leg that traces ObjectChannels snap its feet to the surface. Will regenerate on BeginPlay to snap to dynamically constructed actors like buildings or platforms.

BP_NH_MeshEdgeInstancer
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/be069021-5f85-4ef7-b861-fe5ce7d5e991)

Uses DynamicMesh component and GeometryScript to instantiate meshes using edges from the input StaticMesh, for instancing beams for example. Also places junction meshes on vertices of the input mesh.

BP_NH_PipeActor
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/923631da-a56e-4d73-b57e-81c9be3df377)

Instantiates pre-built pipe pieces StaticMeshes along a spline. Adds corners from Corners StaticMesh array, corners has to be provided in equal intervals from 0 to 180. Various parameters for scaling/offsetting pieces to make it fit. 
It sometimes has gaps but it works very fast and doesn't use SplineMeshes

BP_NH_MultiPipe
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/e23fcb2c-8445-4340-a3d0-f91728667e12)

Instantiates BP_NH_PipeActors along multiple splines provided externally from the level. Used to create sets of multiple cables follow a certain path.

BP_ModelViewer
Tool to go through an array of static meshes, used for inspection.

BP_NH_Platform
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/06aa0144-d50d-47d1-aba9-ac6a809f09a9)

Instantiates floor StaticMeshes along given width and length, separately scales them to specified height. For making floors and platforms.

BP_NH_RadialInstancer
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/512c6c47-4ebc-4360-ac23-85f7eebdadad)

Instantiates StaticMeshes or Actors with given offset from center, start and end degrees, num instances, transformation. Also options to snap to ground, offset pivot.

BP_Satellite and BP_SatelliteTarget
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/ccd39bc3-57e3-4598-8a33-f967d11d2597)

Generates a satelite from modular pieces with options to specify its rotation on construct or during runtime, BP_SatelliteTarget used to focus multiple satelites to a specific point

BP_NH_ShieldRoof
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/612804d8-d9db-4955-8ed7-7b987d971ff5)

Constructs a roof/shield using BP_NH_HydraulicTracedLeg and a spline for length

BP_NH_SnappedCable
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/c0928839-0d2f-43da-a947-93ef09377f2f)

Makes a cable from actor location to specified snap point (actor) with offsets. Also adds specified meshes on each end.

BP_NH_SnapToSplineActor
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/692da555-ec05-4a9d-972c-8ebd57073a12)

Snaps SnappedScene SceneComponent to an externally provided Spline location closest to actor location. For example pipe holding legs or junctions.

BP_NH_SplineInstancer
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/66815073-f1ae-4062-bd87-9d8e331fb9c3)

Distributes meshes along a spline. For example railings or fences.

BP_NH_SplinePlaneMesh
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/492acc8a-1d96-4c01-893e-7d6af828597c)

Uses DynamicMesh and GeometryScript to generate a plane mesh from the shape of a Spline. For example huge planes that go into distance in the back of the level

BP_NH_StaticCableActor and BP_StaticCableActor_ChildSpline
Makes a cable from provided WorldLocations or Spline

BP_NH_TriangulatedScatterer
![image](https://github.com/faxcorp/NewHellDocumentation/assets/37246339/c8cfbaff-ece6-47b8-8163-444dd3aa4f11)

Instantiates StaticMesh or Actors in the area of the spline with provided distances from each other. Can snap to ground. Uses GeometryScript










