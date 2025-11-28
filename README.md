# **Directory Structure and Naming Convention**

A personal standardized naming convention for 3D related projects and Architectural engineering.

## **Table of Contents**

1. [Directory Structure](#directory-structure)
2. [Naming Conventions](#naming-conventions)
3. [Asset Specifications](#asset-specifications)
4. [Code Standards](#code-standards)
5. [Version Control](#version-control)
6. [Performance Guidelines](#performance-guidelines)

---

## **Directory Structure**

```
Assets
├─Art
│  ├─Materials
│  │  ├─MI_Table01_a
│  │  └─MI_Table01_b
│  ├─Models
│  │  ├─Table01_001.blend
│  │  │  └─SM_Table01
│  │  └─Human01_001.blend
│  │     └─Human01
│  └─Textures
│     ├─Table_01_BaseColor.webp
│     ├─Table_01_Normal.png
│     └─Table_01_ORM.webp
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

## **Asset Naming Convention**

Each object that has child data needs to be named without prefixes. For example, an object named `Car01` has a static mesh data named `SM_Car01`. This mesh data should not have instance numbering, so it doesn't need the `[Numbering]` affix. However, the object `Car01` itself can be instanced in some DCC applications, and the naming convention would be written as `Car01_001`. Notice that the `01` after the word "Car" is an archetype number. Another example: material instances like `MI_Table` use texture map data such as `T_Table_Normal.png`. There is a special case for 3D object that has dynamic mesh data, the mesh data in this case should use the object naming convention for example an object of `Human01` has a dynamic mesh data of `Human01`.

Here is how the object naming convention should be written:

```
[NameOfObject][ArchetypeNumbering]_[InstanceNumbering] example: Table01_001
```

And here is how the data naming convention should be written:

```
[Prefix]_[NameOfObject][ArchetypeNumbering] example: SM_Table01
```

#### **Special Cases Naming Convention**

| Type           | Convention                                                   | Example      |
| -------------- | ------------------------------------------------------------ | ------------ |
| `UV`           | UV_[Identifier]_[Numbering]                                  | UV_Map_001   |
| `Dynamic Mesh` | [NameOfObject][ArchetypeNumbering]                           | Human01      |
| `Material`     | MI/M_[NameOfObject][ArchetypeNumbering]_[AlphabeticNumerals] | MI_Table01_a |

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

The naming convention for binaries file should be `[ObjectName].[extension]`
mesh binaries publication,

| Data | Purpose     | .blend | .fbx | .gltf | .obj | notes |
| ---- | ----------- | :----: | :--: | :---: | :--: | ----- |
| mesh | baking      |        |  ✓   |       |      |       |
| mesh | publication |   ✓    |  ✓   |   ✓   |  ✓   |       |

image binaries publication

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
