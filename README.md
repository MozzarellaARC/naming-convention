# **Project Structure and Naming Convention**

A personal standardized naming convention for 3D related projects and Architectural engineering. Although this standardization very verbose, by all means will not replace the naming convention of a basic English. For example a project folder with the purpose to be read by the end user should be written like this `Reference Image`, contrary to the directory structure naming convention where it should be written as `ref` or `reference-image`

## **Table of Contents**

1. [Project Structure](#project-structure)
2. [General Naming Conventions](#general-naming-conventions)
   - [Special Case Naming Convention](#special-case-naming-conventions)
   - [Prefixes by Data Type](#prefixes-by-data-type)
3. [Asset Specifications](#asset-specifications)
   - [Mesh Binaries Publication](#mesh-binaries-publication)
   - [Image Binaries Publication](#image-binaries-publication)
4. [Footnote](#footnote)

---

## **Data Structure**
In most DCC application, it is common for a data comprise of object and the actual data, for example a data of object `Apple` contains a mesh data of `Apple` and material data of `MI_AppleRed`. This object data content can change, like for example an object of `Apple` may have mesh data of `Orange` and material data of `MI_BananaYellow`.

Now the naming convention for this data structure in general use `PascalCase` while the binary data that is referenced by the object use `Title_Snake_Case`:

Apple
├─Apple -> mesh data
└─MI_AppleRed -> material instance data
    └─T_AppleRed_Normal.jpeg -> image binary data

The prefix T_ and the suffix _Normal will be explain later on in the documentation. Please keep in mind that each DCC has different patter when dealing with instances, like for example duplicating `Apple` object inside Unity engine will give (n) suffix while in Blender gives .n suffix.
```
Unity
Apple (1)
Apple (2)
etc.

Blender
Apple.001
Apple.002
etc.
```

## **Directory Structure**
The naming convention for the directory use `kebab-case`, the use of whitespace to name directories are forbidden because most string parser will ignore whitespaces, which makes using whitespaces redundant and harder to organize with a script. So if I have a folder with name User Application, it should be written as `user-application`. Although the top level directory uses `kebab-case` using the same convention for the sub-folder might be confusing, so the recommended way to naming the sub-folder is by using single wording term like for example use `ref` instead of `reference-image`. Binary data filenames (like textures, models, and reference materials) should use `Title_Snake_Case` regardless of their folder location. This is how the directory structure would look like:

```
example-project
├─README.md
├─workspace.blend
├─publish
├─lib
│  ├─models
│  │  ├─Table01.blend -> binary
│  │  │  └─SM_Table01 -> binary data
│  │  └─Human01_001.blend -> binary
│  │     └─Human01 -> binary data
│  ├─textures
│  │  ├─T_Table01_BaseColor.webp -> binary data
│  │  ├─T_Table01_Normal.png -> binary data
│  │  └─T_Table01_ORM.webp -> binary data
│  ├─texturing -> substance painter or marmoset toolbag
│  │  └─Table_Texturing.spp
│  └─exports
│     └─Table01.fbx
├─ref
│  ├─PerforatedMetal.jpg -> binary data
│  └─Dimensions.pdf -> binary data
└─scripts
    ├─PlayerController.cs -> C# standard naming convention
    └─Scripts.cs -> C# standard naming convention
```

## **Project Structure**

```
Assets
├─Art -> should be the unique identifier
│  ├─Materials
│  │  ├─Table01_a -> suffix _a is a material sequence
│  │  │  └─T_Table01_a_BaseColor.webp
│  │  └─Table01_b -> suffix _b is a material sequence
│  ├─Models
│  │  ├─Table01.blend
│  │  │  └─Table01
│  │  │     └─SM_Table01
│  │  └─Human01.blend
│  │     └─Human01 -> object
│  │        └─Human01 -> mesh data
│  └─Textures
│     ├─T_Table01_BaseColor.webp
│     ├─T_Table01_Normal.png
│     └─T_Table01_ORM.webp
├─Audio
│  ├─Music
│  │  └─Background.wav
│  └─Sound
└─Code
   ├─Scripts
   │  ├─PlayerController.cs
   │  └─Scripts.cs
   └─Shaders
      └─MaterialShaders.hlsl
```

---

## **General Naming Conventions**

The naming convention for working DCC object data uses `PascalCase`, while binary data files use `Title_Snake_Case`. Objects and their child data follow a hierarchical structure where prefixes identify data types.

Each object that has child data needs to be named with appropriate prefixes. For example, an object named `Car01` has a static mesh data named `SM_Car01`. This mesh data should not have instance numbering, so it doesn't need the `[Numbering]` affix. However, the object `Car01` itself can be instanced in some DCC applications, and the naming convention would be written as `Car01_001`. Notice that the `01` after the word "Car" is an archetype number. Another example: material instances like `MI_Table` use texture map data such as `T_Table_Normal.png`. There is a special case for 3D object that has dynamic mesh data, the mesh data in this case should use the object naming convention for example an object of `Human01` has a dynamic mesh data of `Human01`.

Here is how the object naming convention should be written:

```
[ObjectName][ArchetypeNumbering]_[InstanceNumbering] example: Table01_001
```

And here is how the data naming convention should be written:

```
[Prefix]_[ObjectName][ArchetypeNumbering] example: SM_Table01
```

#### **Special Case Naming Conventions**

| Type           | Convention                                                 | Example      |
| -------------- | ---------------------------------------------------------- | ------------ |
| `UV`           | UV_[Identifier]_[Numbering]                                | UV_Map_001   |
| `Dynamic Mesh` | [ObjectName][ArchetypeNumbering]                           | Human01      |
| `Material`     | MI/M_[ObjectName][ArchetypeNumbering]_[AlphabeticNumerals] | MI_Table01_a |

#### **Prefixes by Data Type**

| Prefix  | Type              | Example                  |
| ------- | ----------------- | ------------------------ |
| `SM_`   | Static Mesh       | SM_Building_Skyscraper01 |
| `SK_`   | Skeletal Mesh     | SK_Character_Detective   |
| `ANIM_` | Animation         | ANIM_HumanRunForward     |
| `MI_`   | Material Instance | MI_Metal_RustySteel      |
| `M_`    | Material (Master) | M_Standard_PBR           |
| `T_`    | Texture           | T_Concrete_BaseColor     |
| `UV_`   | UV                | UV_Map                   |
| `A_`    | Audio             | A_Background             |

---

## **Asset Specifications**

Binary files should use `Title_Snake_Case` naming convention with the format `[ObjectName].[Extension]`. There is also a special case for mesh binaries that is targeted to baking pipeline where each mesh related to their complexity needs to be given suffix `_Low`, `_High`, `_Cage` for example `Table_Low.fbx`. Although some DCC can read the mesh data on their interface, for the most part the mesh data does not need to be given unique identifier.

#### **Mesh Binaries Publication**

| Data | Purpose     | .blend | .fbx | .gltf | .obj | notes |
| ---- | ----------- | :----: | :--: | :---: | :--: | ----- |
| mesh | baking      |        |  ✓   |       |      |       |
| mesh | publication |   ✓    |  ✓   |   ✓   |  ✓   |       |

#### **Image Binaries Publication**

| Data                   | Purpose     | .png | .jpg | .webp | notes           |
| ---------------------- | ----------- | :--: | :--: | :---: | --------------- |
| image                  | reference   |      |  ✓   |       | any, optional   |
| BaseColor              | publication |      |      |   ✓   | 8bits           |
| Normal map             | publication |  ✓   |      |       | 16bits, OpenGL  |
| Roughness map          | publication |      |      |   ✓   | 8bits           |
| Metalness map          | publication |      |      |   ✓   | 8bits           |
| ORM map                | publication |      |      |   ✓   | 8bits           |
| MetallicSmoothness map | publication |      |      |   ✓   | 8bits           |
| Opacity map            | publication |      |      |   ✓   | 8bits, optional |
| Emissive map           | publication |      |      |   ✓   | 8bits, optional |

---

#### *Footnote*
Naming Convention are based off of Unity and Unreal Engine assets naming convention:

[Unreal Engine Recommended Asset Naming Convention](https://dev.epicgames.com/documentation/en-us/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects)\
[Best practices for organizing your Unity project](https://unity.com/how-to/organizing-your-project)

## Architecture

When we explicitly refer to *“Architecture”* we mean the professional discipline of building design, rather than the concept of “architecture” as used in computer science.

For architectural projects, naming conventions should be as human-readable as possible. Binary files and related data should follow the structure:

[Noun][Whitespace][Adjective][Whitespace][Extra-Adjective]

Every first letter of each identifier should be capitalized. If the [Adjective] contains multiple words, it must use a hyphen - as punctuation. The adjectives should be ordered from the most general category to the more specific. Below is an example of this convention for architecture-related binaries and data:

```Power Socket Type-F```

In this example, `Power` is the Noun, `Socket` is the general-category Adjective, and `Type-F` is the specific-category [ Extra-Adjective ]. Notice that `Type-F` comes after `Socket` because Socket identifies the broader category. At this category level, there are only two possibilities: `Socket` (power-source input) and `Plug` (power-source output), while connector types can vary.
