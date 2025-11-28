# **Directory Structure and Naming Convention**

A personal standardized naming convention for 3D related projects and Architectural engineering.

## **Table of Contents**

1. [Directory Structure](#directory-structure)
2. [General Naming Conventions](#general-naming-conventions)
   - [Special Case Naming Convention](#special-case-naming-conventions)
   - [Prefixes by data type](#prefixes-by-data-type)
3. [Asset Specifications](#asset-specifications)
   - [Mesh Binaries Publication](#mesh-binaries-publication)
   - [Image binaries publication](#image-binaries-publication)
4. [Footnote](#footnote)

---

## **Directory Structure**

```
Assets
├─README.md
├─workspace.blend
├─Art
│  ├─Materials
│  │  ├─MI_Table01_a
│  │  │  └─T_Table_01_BaseColor.webp
│  │  └─MI_Table01_b
│  ├─Models
│  │  ├─Table01_001.blend
│  │  │  └─SM_Table01
│  │  └─Human01_001.blend
│  │     └─Human01
│  └─Textures
│     ├─T_Table_01_BaseColor.webp
│     ├─T_Table_01_Normal.png
│     └─T_Table_01_ORM.webp
├─Audio
│  ├─Music
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

The naming convention for the working DCC related file should be:
```
[ObjectName].[Extension]
```

Each object that has child data needs to be named without prefixes. For example, an object named `Car01` has a static mesh data named `SM_Car01`. This mesh data should not have instance numbering, so it doesn't need the `[Numbering]` affix. However, the object `Car01` itself can be instanced in some DCC applications, and the naming convention would be written as `Car01_001`. Notice that the `01` after the word "Car" is an archetype number. Another example: material instances like `MI_Table` use texture map data such as `T_Table_Normal.png`. There is a special case for 3D object that has dynamic mesh data, the mesh data in this case should use the object naming convention for example an object of `Human01` has a dynamic mesh data of `Human01`.

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
| `ANIM_` | Animation         | ANIM_Human_Run_Forward   |
| `MI_`   | Material Instance | MI_Metal_RustySteel      |
| `M_`    | Material (Master) | M_Standard_PBR           |
| `T_`    | Texture           | T_Concrete_BaseColor     |
| `UV_`   | UV                | UV_Map                   |

---

## **Asset Specifications**

The naming convention for binaries file should be `[ObjectName].[Extension]`. There is also a special case for mesh binaries that is targeted to baking pipeline where each mesh related to their complexity needs to be given suffix `_low` , `_high` , `_cage` for example `table_low`. Although some DCC can read the mesh data on their interface, for the most part the mesh data does not need to be given unique identifier.

#### **mesh binaries publication**

| Data | Purpose     | .blend | .fbx | .gltf | .obj | notes |
| ---- | ----------- | :----: | :--: | :---: | :--: | ----- |
| mesh | baking      |        |  ✓   |       |      |       |
| mesh | publication |   ✓    |  ✓   |   ✓   |  ✓   |       |

#### **image binaries publication**

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
