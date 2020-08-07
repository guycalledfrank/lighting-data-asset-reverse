# Reverse-engineering Lighting Data Assets

Lighting data assets are little annonying mysteries. Unity uses them to store:
* Baked lightmap list (https://docs.unity3d.com/ScriptReference/LightmapSettings-lightmaps.html)
* Some per-light settings (at least the LightBakingOutput structure: https://docs.unity3d.com/ScriptReference/LightBakingOutput.html)
* Some per-renderer settings (notably lightmapIndex and lightScaleOffset: https://docs.unity3d.com/ScriptReference/Renderer-lightmapIndex.html)
* Light probes (also accessible via API: https://docs.unity3d.com/ScriptReference/LightProbes.html)
* Occlusion probes ("shadowmasks" for light probes; mostly not accessible via API)
* Special Enlighten data.
* Probably something else.

Unity does not want to open the format or provide more read/write APIs for the asset (https://forum.unity.com/threads/lighting-data-asset-was-a-mistake.547591), so some reverse-engineering can be useful.

Unfortunately I'm not really good at it, so mainly I'm creating this repository as a starting point. Hopefully other people will join and expand this knowledge.

What is currently known:

* Lighting data asset format can differ from version to version, but the differences are usually minor and the engine is often able to load older assets.

* The asset is likely using some general data packing scheme applicable to other assets as well.

* First part of the file contains unknown data (general serialization stuff?). But if you look for the "LightingData" string, surrounding bytes might make sense.

If you have Hex Workshop, you can use my Structure file (ldata.hsl) to help viewing the assets. If not, you can still read it as a format spec.
Notes on Structure files: http://www.hexworkshop.com/onlinehelp/500/html/idhelp_structure_libraries.htm

The file is based on Unity 5.6 lighting data asset format. There are likely some changes in newer versions.

There are likely some mistakes.

Unknown non-reversed area of the file contains some checksums/data lengths that must be patched if you change something else. Exact locations of these checksums are hard to tell, but you can see them changing if you diff two files with a different amount of something.
