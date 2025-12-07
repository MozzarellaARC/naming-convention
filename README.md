# **Preface**

When dealing with namespaces or naming conventions, the rules can vary depending on the platform. For example, the Windows file system is case-insensitive by default but case-preserving. This means that `AppleRed` and `applered` refer to the same file or folder, even though Windows remembers the original casing. This behavior can make namespace organization more difficult, because separating words such as Apple and Red cannot rely on case differences or punctuation alone and instead requires more complicated parsing rules.

With all of this in mind, the community uses `PascalCase` or `PascalCase` with affixes when naming things. We should not forget about human readability and cross-platform collision prevention.

## **Table of Contents**

1. [Data Structure](#data-structure)
2. [Directory Structure](#directory-structure)
3. [Project Structure](#project-structure)
4. [General Naming Conventions](#general-naming-conventions)
   - [Special Case Naming Conventions](#special-case-naming-conventions)
   - [Prefixes by Data Type](#prefixes-by-data-type)
5. [Asset Specifications](#asset-specifications)
   - [Mesh Binaries Publication](#mesh-binaries-publication)
   - [Image Binaries Publication](#image-binaries-publication)
6. [Architecture Projects](#architecture-projects)
7. [Footnote](#footnote)

---

## **Data Structure**

In most DCC applications, it is common for an object to reference several data blocks—for example, an object named `Apple` may reference mesh data also named `Apple` and material data named `MI_AppleRed`. Some DCCs allow these data block names to be mismatched; for example, an object named `Apple` could use mesh data named `Orange` and material data named `MI_BananaYellow`. However, naming conventions should not depend on this permissiveness.

In general, object names typically use `PascalCase`, while the underlying data blocks referenced by those objects also use `PascalCase` with affixes applied, such as prefixes for meshes and materials.

The naming convention for this data structure uses `PascalCase` for objects while the data blocks referenced by the object use `PascalCase` with added prefixes:

```
Apple
├─SM_Apple → static mesh data
└─MI_AppleRed → material instance data
    └─T_AppleRed_Normal.png → texture binary data
```

The prefix `T_` identifies texture data, and the suffix `_Normal` indicates a normal map texture type (see [Image Binaries Publication](#image-binaries-publication) for all texture suffixes).

Please keep in mind that each DCC applies its own pattern when handling duplicated instances; for example, duplicating the `Apple` object in Unity results in a `(1)`, `(2)`, `(3)` suffix, while Blender uses `.001`, `.002`, `.003`. Here is the example:

```
Unity:
Apple (1)
Apple (2)
etc.

Blender:
Apple.001
Apple.002
etc.
```

**Note:** These auto-generated instance suffixes are DCC-specific behaviors and are not part of this naming convention standard.

---

## **Directory Structure**

The naming convention for the top-level project directory uses `kebab-case`. The use of whitespace to name directories is problematic because some string parsers ignore whitespaces, which makes using whitespaces redundant and harder to organize with scripts. So if, for example, a folder is named "User Application", it should be written as `user-application`.

Although the top-level directory uses `kebab-case` for clarity and project identity, applying the same rule to sub-folders creates unnecessary visual noise and complicates script-based parsing of directory paths. Sub-folders represent categorical namespaces (e.g., `ref`, `lib`, `scripts`) and rarely require multi-word descriptors. Using short, single-term names keeps the hierarchy easy to scan and reduces ambiguity in automated tooling. Therefore, the recommended way of naming sub-folders is by using single-word terms—for example, use `ref` instead of `reference-image`.

Binary data filenames (like textures, models, and reference materials) should use `PascalCase` regardless of their folder location. This is how the directory structure would look:

```
example-project
├─README.md
├─workspace.blend
├─publish
├─lib
│  ├─models
│  │  ├─Table01.blend → binary
│  │  │  └─SM_Table01 → static mesh data
│  │  └─Human01.blend → binary
│  │     └─SK_Human01 → skeletal mesh data
│  ├─textures
│  │  ├─T_Table01_BaseColor.webp → binary data
│  │  ├─T_Table01_Normal.png → binary data
│  │  └─T_Table01_ORM.webp → binary data
│  ├─texturing → substance painter or marmoset toolbag
│  │  └─TableTexturing.spp
│  └─exports
│     └─Table01.fbx
├─ref
│  ├─*.jpg → binary data reference
│  └─*.pdf → binary data reference
└─scripts
    ├─PlayerController.cs → C# standard naming convention
    └─Scripts.cs → C# standard naming convention
```

---

## **Project Structure**

This project structure is designed specifically for the Unity Game Engine:

```
Assets
├─Art → should be the unique identifier
│  ├─Materials
│  │  ├─Table01_a → suffix _a is a material sequence
│  │  │  └─T_Table01_a_BaseColor.webp
│  │  └─Table01_b → suffix _b is a material sequence
│  ├─Models
│  │  ├─Table01.blend
│  │  │  └─Table01
│  │  │     └─SM_Table01
│  │  └─Human01.blend
│  │     └─Human01 → object
│  │        └─SK_Human01 → skeletal mesh data
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

The naming convention for DCC-specific object data and binaries uses `PascalCase`. Objects and their child data follow a hierarchical structure where prefixes identify data types.

Child data blocks must be named with appropriate prefixes. For example, an object named `Car01` has static mesh data named `SM_Car01`. Notice that the `01` after the word "Car" is an archetype number that identifies which design variant of "Car" this is (e.g., `Car01` might be a sedan, `Car02` might be a truck).

**Instance vs. Archetype Numbering:**
- **Archetype numbering** (e.g., `01`, `02`) identifies different design variants and is part of the naming convention
- **Instance numbering** (e.g., `_001`, `_002`) identifies duplicates of the same design and may be used for scene objects but not for data blocks

Another example: Material instances like `MI_Table01_a` use texture map data such as `T_Table01_Normal.png`. 

**Special case for skeletal meshes:** Objects with skeletal mesh data (rigged characters or animated objects) use the `SK_` prefix. For example, an object named `Human01` has skeletal mesh data named `SK_Human01`. Skeletal meshes differ from static meshes because they contain bone/armature data and are intended for animation.

### **Object Naming Convention**

```
[ObjectName][ArchetypeNumbering]_[InstanceNumbering]
```

**Example:** `Table01_001`
- `Table` = object name
- `01` = archetype number (design variant)
- `_001` = instance number (optional, for scene duplicates)

### **Data Block Naming Convention**

```
[Prefix]_[ObjectName][ArchetypeNumbering]
```

**Example:** `SM_Table01`
- `SM_` = prefix (static mesh)
- `Table` = object name
- `01` = archetype number

Data blocks always include the archetype number to match their parent object, but never include instance numbering.

---

### **Special Case Naming Conventions**

| Type       | Convention                                                 | Example      |
| ---------- | ---------------------------------------------------------- | ------------ |
| `UV`       | UV_[Identifier]_[NumericalSequence]                        | UV_Map_001   |
| `Material` | MI/M_[ObjectName][ArchetypeNumbering]_[AlphabeticSequence] | MI_Table01_a |

**Material Sequence Explanation:** The alphabetic sequence (e.g., `_a`, `_b`, `_c`) is used when a single object has multiple material variations. For example, `MI_Table01_a` might be a wood finish, while `MI_Table01_b` might be a metal finish for the same table design.

---

### **Prefixes by Data Type**

| Prefix  | Type              | Example                  |
| ------- | ----------------- | ------------------------ |
| `SM_`   | Static Mesh       | SM_BuildingSkyscraper01  |
| `SK_`   | Skeletal Mesh     | SK_Character_Detective   |
| `ANIM_` | Animation         | ANIM_HumanRunForward     |
| `MI_`   | Material Instance | MI_MetalRustySteel       |
| `M_`    | Material (Master) | M_Standard_PBR           |
| `T_`    | Texture           | T_Concrete_BaseColor     |
| `UV_`   | UV Map            | UV_Map_001               |
| `A_`    | Audio             | A_Background             |

---

## **Asset Specifications**

Binary files should use `PascalCase` with the format `[ObjectName][ArchetypeNumbering].[Extension]`.

**Special case for baking pipelines:** Mesh files used in texture baking workflows require complexity-level suffixes: `_low`, `_high`, `_cage`. For example: `Table01_low.fbx`, `Table01_high.fbx`, `Table01_cage.fbx`.

---

### **Mesh Binaries Publication**

| Data | Purpose     | .blend | .fbx | .gltf | .obj | Notes |
| ---- | ----------- | :----: | :--: | :---: | :--: | ----- |
| Mesh | Baking      |        |  ✓   |       |      |       |
| Mesh | Publication |   ✓    |  ✓   |   ✓   |  ✓   |       |

---

### **Image Binaries Publication**

Texture files follow this naming pattern:

```
T_[ObjectName][ArchetypeNumbering]_[TextureType].[Extension]
```

**Example:** `T_Table01_BaseColor.webp`

| Texture Type       | Purpose     | .png | .jpg | .webp | Notes                    |
| ------------------ | ----------- | :--: | :--: | :---: | ------------------------ |
| Reference          | Reference   |      |  ✓   |       | Any format, optional     |
| BaseColor          | Publication |      |      |   ✓   | 8-bit sRGB               |
| Normal             | Publication |  ✓   |      |       | 16-bit linear, OpenGL    |
| Roughness          | Publication |      |      |   ✓   | 8-bit linear             |
| Metalness          | Publication |      |      |   ✓   | 8-bit linear             |
| ORM                | Publication |      |      |   ✓   | 8-bit (Occlusion/Roughness/Metalness) |
| MetallicSmoothness | Publication |      |      |   ✓   | 8-bit                    |
| Opacity            | Publication |      |      |   ✓   | 8-bit linear, optional   |
| Emissive           | Publication |      |      |   ✓   | 8-bit sRGB, optional     |

**Texture Type Suffixes:**
- `_BaseColor` - Albedo/diffuse color map
- `_Normal` - Normal map for surface detail
- `_Roughness` - Surface roughness (grayscale)
- `_Metalness` - Metallic properties (grayscale)
- `_ORM` - Combined Occlusion, Roughness, Metalness (R=Occlusion, G=Roughness, B=Metalness)
- `_MetallicSmoothness` - Combined metallic and smoothness for Unity
- `_Opacity` - Transparency map
- `_Emissive` - Self-illumination map

---

## **Architecture Projects**

When we explicitly refer to *"Architecture"* or *Architectural*, we mean the professional discipline of building design, rather than the concept of "architecture" as used in computer science.

For architectural projects, naming conventions should prioritize human readability over strict technical formatting. Binary files and related data should follow the structure:

```
[Noun] [Adjective] [Extra-Adjective]
```

Every first letter of each word should be capitalized. If the `[Adjective]` or `[Extra-Adjective]` contains multiple words, it must use a hyphen (`-`) as punctuation. The adjectives should be ordered from the most general category to the more specific.

**Example:** `Power Socket Type-F`

Breakdown:
- `Power` = Noun (the main object category)
- `Socket` = Adjective (general category - input device)
- `Type-F` = Extra-Adjective (specific variant - European standard)

**Rationale:** `Socket` comes before `Type-F` because Socket identifies the broader category (there are only two possibilities: `Socket` for power-source input and `Plug` for power-source output), while connector types can vary widely (Type-A, Type-F, Type-G, etc.).

**Additional examples:**
- `Window Frame Double-Glazed`
- `Door Handle Lever-Style`
- `Light Fixture Recessed-LED`

---

## **Footnote**

This naming convention is based on the Rust language API guidelines, Unity Engine, and Unreal Engine asset naming conventions:

- [Rust Api Guidelines for Naming](https://rust-lang.github.io/api-guidelines/naming.html)
- [Rust Naming Conventions](https://github.com/rust-lang/rfcs/blob/master/text/0430-finalizing-naming-conventions.md)
- [Unreal Engine Recommended Asset Naming Convention](https://dev.epicgames.com/documentation/en-us/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects)
- [Best practices for organizing your Unity project](https://unity.com/how-to/organizing-your-project)
