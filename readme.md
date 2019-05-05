# dlaf-optimized

Optimization of [Michael Fogleman](https://github.com/fogleman/dlaf)'s dlaf project that I used in [**glChAoS.P / wglChAoS.P**](https://github.com/BrutPitt/glChAoS.P) for large-scale DLA3D growth.

[![](https://raw.githubusercontent.com/BrutPitt/glChAoS.P/master/screenShots/dla3D.jpg)](https://twitter.com/i/status/1120431893818769409)



All the changes I've made in [**glChAoS.P / wglChAoS.P**](https://github.com/BrutPitt/glChAoS.P) , I thought I'd make them available here, for simplicity.

### Main changes
- Possibility to use for spatial index the *single file header* [nanoflann](https://github.com/jlblancoc/nanoflann) (Fast Library for Approximate Nearest Neighbors), instead of boost library.
- The use of [fastRandomGenerator](https://github.com/BrutPitt/fastRandomGenerator), based on George Marsaglia algorithm, to speedup the generation.
- The use of `template class` for `Vector` and `Model` with possibilty to use single or double precison floating points.

The [Michael Fogleman](https://github.com/fogleman/dlaf)'s original option (as boost library, or std::mt19937 random generator) are anyway aviables via `#define` 

**External Dependences**

**NONE**: all files are already included in the project/repository

- `nanoflann.h` -> header-only library for KD-Trees of datasets point clouds: [nanoflann](https://github.com/jlblancoc/nanoflann)
- `fastRandom.h` -> fastRandomGeneretor based on Marsaglia algorithms: [fastRandomGenerator](https://github.com/BrutPitt/fastRandomGenerator)

If you want use Boost library it's not included in the repository, but can be downloaded from [https://www.boost.org/](https://www.boost.org/)
It is not necessary to build the library, only headers files is enough. (about 160MB) 

### Output Format
This program produces only a simple text output, for both for 2D and 3D, always in the form:
```bash
0,-1,0,0,0
1,0,-0.191125,-0.446429,0.87417
2,1,0.697415,-0.821024,1.13908
3,0,-0.464858,0.327307,-0.822665
4,2,1.63213,-0.595306,0.86457
5,1,-1.06683,-0.335015,1.34398
6,5,-1.88992,-0.368307,1.91091
7,6,-2.20384,0.449373,2.39347
...
```
Where the columns are: point_id, parent_id, x, y, z (for 2D z is always 0)

If You want export a DLA object in PLY file format (ascii/binary), please use [**glChAoS.P / wglChAoS.P**](https://github.com/BrutPitt/glChAoS.P); you can also import a PLY object, where to grow the DLA:

| Thomas Attractor | DLA on Thomas Attractor |
| :-----: | :----: |
|[![](https://raw.githubusercontent.com/BrutPitt/glChAoS.P/master/screenShots/osSS01.jpg)](https://michelemorrone.eu/glchaosp/screenshots.html#osSShots) | [![](https://raw.githubusercontent.com/BrutPitt/glChAoS.P/master/screenShots/ss001.jpg)](https://michelemorrone.eu/glchaosp/screenshots.html#osSShots)|


**Hooks & Parameters** form [Michael Fogleman](https://github.com/fogleman/dlaf)'s page

The code implements a standard diffusion-limited aggregation algorithm. But there are several parameters and code hooks that let you tweak its behavior.

The following parameters can be set on a `Model` instance.

| Parameter | Description |
| --- | --- |
| `AttractionDistance` | Defines how close together particles must be in order to join together. |
| `ParticleSpacing` | Defines the distance between particles when they become joined together. |
| `MinMoveDistance` | Defines the minimum distance that a particle will move in an iteration during its random walk. |
| `Stubbornness` | Defines how many join attempts must occur before a particle will allow another particle to join to it. |
| `Stickiness` | Defines the probability that a particle will allow another particle to join to it. |

The following hooks allow you to define the algorithm behavior in small, well-defined functions.

| Hook | Description |
| --- | --- |
| `RandomStartingPosition()` | Returns a starting position for a new particle to begin its random walk. |
| `ShouldReset(p)` | Returns true if the particle has gone too far away and should be reset to a new random starting position. |
| `ShouldJoin(p, parent)` | Returns true if the point should attach to the specified parent particle. This is only called when the point is already within the required attraction distance. If false is returned, the particle will continue its random walk instead of joining to the other particle. |
| `PlaceParticle(p, parent)` | Returns the final placement position of the particle. |
| `MotionVector(p)` | Returns a vector specifying the direction that the particle should move for one iteration. The distance that it will move is determined by the algorithm. |


