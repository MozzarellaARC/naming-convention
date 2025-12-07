# **Preface**

When dealing with namespaces or naming conventions, the rules can vary depending on the platform. For example, the Windows file system is case-insensitive by default but case-preserving. This means that `AppleRed` and `applered` refer to the same file or folder, even though Windows remembers the original casing. This behavior can make namespace organization more difficult, because separating words such as Apple and Red cannot rely on case differences or punctuation alone and instead requires more complicated parsing rules.

With all of this in mind, the community uses `PascalCase` or `PascalCase` with affixes when naming things. We should not forget about human readability and cross-platform collision prevention.

## **Table of Contents**

1. [General Naming Conventions](#general-naming-conventions)
2. [Asset Workspace](#asset-workspace)
3. [Project Structure](#project-structure)
4. [Data Structure](#data-structure)
5. [Asset Specifications](#asset-specifications)
   - [Mesh Binaries Publication](#mesh-binaries-publication)
   - [Image Binaries Publication](#image-binaries-publication)
6. [Architecture Projects](#architecture-projects)
7. [Footnote](#footnote)

---

## **1. General Naming Conventions**

This documentation uses `PascalCase` as the foundation for all naming conventions. Objects and their associated data blocks follow a hierarchical relationship where prefixes and suffixes provide type identification and variant information.

### **a. Archetype vs. Instance Numbering:**
- Archetype numbering (e.g., `01`, `02`) - Identifies different design variants of the same object type. Example: `Car01` might be a sedan while `Car02` is a truck.
- Instance numbering: (e.g., `_001`, `_002`) - Identifies duplicate copies of the same archetype within a scene. Used for objects but not for data blocks.

### **b. Hierarchical Data Relationship:**
Objects serve as containers that reference underlying data blocks. These data blocks use prefixes to identify their type (mesh, material, texture, etc.) and maintain a naming relationship with their parent object.

### **c. Naming Patterns:**
- Objects - Use descriptive names with archetype numbers and optional instance suffixes
- Data Blocks - Use prefixes followed by the parent object's name and archetype number
- Binaries - Use the same naming as data blocks with appropriate file extensions

**Quick Reference:**

| Component    | Uses Archetype | Uses Instance | Example          |
| ------------ | -------------- | ------------- | ---------------- |
| Object       | ✓              | ✓ (optional)  | Table01_001      |
| Data Block   | ✓              | ✗             | SM_Table01       |
| Binary File  | ✓              | ✗             | Table01.blend    |

---

## **2. Asset Workspace**

This section describes the folder organization for asset creation projects—the source workspace where artists and technical artists develop 3D models, textures, and related content before importing them into a game engine or other application.

The naming convention for the top-level project directory uses `kebab-case`. The use of whitespace to name directories is problematic because some string parsers ignore whitespaces, which makes using whitespaces redundant and harder to organize with scripts. So if, for example, a folder is named "User Application", it should be written as `user-application`.

Although the top-level directory uses `kebab-case` for clarity and project identity, applying the same rule to sub-folders creates unnecessary visual noise and complicates script-based parsing of directory paths. Sub-folders represent categorical namespaces (e.g., `ref`, `lib`, `scripts`) and rarely require multi-word descriptors. Using short, single-word names keeps the hierarchy easy to scan and reduces ambiguity in automated tooling. Therefore, the recommended way of naming sub-folders is by using single-word terms—for example, use `ref` instead of `reference-image`.

Binary data filenames use `PascalCase` regardless of their folder location. This is how an asset creation project directory structure would look:

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

## **3. Project Structure**

This section demonstrates the standard folder organization within a Unity Game Engine project. Unlike the [Asset Workspace](#asset-workspace) which organizes source asset creation files, the Project Structure represents how assets are organized within Unity's `Assets` folder after import.

Unity projects follow engine-specific conventions where directories use either `PascalCase` or single-term names with capitalized first letters (e.g., `Art`, `Audio`, `Code`), rather than the `kebab-case` used in source asset directories.

**Key Differences:**
- **Asset Workspace** = source asset creation workspace (uses `kebab-case` for top-level, single terms for sub-folders)
- **Project Structure** = Unity's `Assets` folder organization after import (uses `PascalCase`)

The example below shows how assets are categorized by type within Unity's `Assets` directory. This structure is widely adopted across Unity projects for consistency and ease of navigation:

```
Assets
├─Art → primary content folder (models, materials, textures)
│  ├─Materials
│  │  ├─Table01_a → material variant A
│  │  │  └─T_Table01_a_BaseColor.webp
│  │  └─Table01_b → material variant B
│  ├─Models
│  │  ├─Table01.blend
│  │  │  └─Table01 → object container
│  │  │     └─SM_Table01 → static mesh data
│  │  └─Human01.blend
│  │     └─Human01 → object container
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

**Note:** The `Art` folder serves as the primary identifier for visual content. Some teams may use alternative top-level names like `Graphics`, `Visuals`, or `Game` depending on project conventions.

---

## **4. Data Structure**

### **a. Binary Naming Conventions**

Binary files follow `PascalCase` naming conventions. When working with assets that have multiple variants or archetypes, append a two-digit numeric suffix directly to the name without any separator. For assets that require additional identifiers beyond the base name, use an underscore (`_`) to separate these identifiers.

**Optional Versioning:** Version numbers can be added for specific workflow scenarios (such as iterative prototypes or archived assets). When used, they should be separated by underscores and follow semantic versioning format (MAJOR.MINOR.PATCH). This is not required for standard production assets.

**Binary Format Examples**

| Type                                  | Format                                   | Example                             |
| ------------------------------------- | ---------------------------------------- | ----------------------------------- |
| Simple assets                         | `AssetName.extension`                    | `ApplePie.blend`                    |
| Assets with archetypes/variants       | `AssetName##.extension`                  | `ApplePie01.blend`                  |
| Assets with identifiers               | `AssetName##_Identifier.extension`       | `ApplePie01_Prototype.blend`        |
| Versioned assets (optional)           | `AssetName##_Identifier_vX.Y.Z.extension`| `ApplePie01_Prototype_v1.0.0.blend` |

**Note:** The `##` represents a two-digit archetype number (01, 02, 03, etc.).

---

### **b. Object and Data Block Structure**

In most DCC applications, an object references several data blocks. For example, an object named `Apple01` may reference mesh data `SM_Apple01` and material data `MI_Apple01_a`:

```
Apple01
├─SM_Apple01 → static mesh data
└─MI_Apple01_a → material instance data
    └─T_Apple01_BaseColor.webp → texture binary data
```

**Object Naming Pattern**

```
ObjectName##_###
```

**Example:** `Table01_001`
- `Table` = object name
- `01` = archetype number (design variant)
- `_001` = instance number (optional, for scene duplicates)

**Data Block Naming Pattern**

```
Prefix_ObjectName##
```

**Example:** `SM_Table01`
- `SM_` = prefix (static mesh)
- `Table` = object name
- `01` = archetype number

Child data blocks must be named with appropriate prefixes. An object named `Car01` has static mesh data named `SM_Car01`. Data blocks always include the archetype number to match their parent object, but never include instance numbering.

**Special case for skeletal meshes:** Objects with rigged/animated mesh data use the `SK_` prefix. For example, `Human01` has skeletal mesh data `SK_Human01`. Skeletal meshes contain bone/armature data for animation, unlike static meshes which are rigid.

---

### **c. DCC Instance Suffixes**

Each DCC auto-generates instance suffixes differently when duplicating objects:

```
Unity:
Apple (1)
Apple (2)

Blender:
Apple.001
Apple.002
```

**Note:** These auto-generated suffixes are DCC-specific behaviors and not part of this naming standard.

---

### **7. Special Case Naming Conventions**

| Type       | Format                            | Example      | Notes                                                                    |
| ---------- | --------------------------------- | ------------ | ------------------------------------------------------------------------ |
| `UV`       | `UV_Identifier_###`               | `UV_Map_001` | Identifier typically describes UV purpose (Map, Lightmap, etc.)          |
| `Material` | `MI/M_ObjectName##_AlphaSequence` | `MI_Table01_a` | Alphabetic suffix (_a, _b, _c) identifies material variations |

**Material Sequence:** Alphabetic suffixes (`_a`, `_b`, `_c`) identify material variations for the same object. Example: `MI_Table01_a` = wood finish, `MI_Table01_b` = metal finish.

**UV Maps:** The numerical sequence allows multiple UV channels for the same object. The identifier describes the UV purpose (e.g., `UV_Lightmap_001` for lightmap UVs, `UV_Map_001` for standard texture coordinates).

---

## **10. Prefixes by Data Type**

| Prefix  | Type              | Example                  |
| ------- | ----------------- | ------------------------ |
| `SM_`   | Static Mesh       | `SM_BuildingSkyscraper01`  |
| `SK_`   | Skeletal Mesh     | `SK_Character_Detective`   |
| `ANIM_` | Animation         | `ANIM_HumanRunForward`     |
| `MI_`   | Material Instance | `MI_MetalRustySteel`       |
| `M_`    | Material (Master) | `M_Standard_PBR`           |
| `T_`    | Texture           | `T_Concrete_BaseColor`     |
| `UV_`   | UV Map            | `UV_Map_001`               |
| `A_`    | Audio             | `A_Background`             |

---

## **9. Asset Specifications**

This section defines the technical specifications for mesh and image binary exports.

**Special case for baking pipelines:** Mesh files used in texture baking workflows require complexity-level suffixes: `_low`, `_high`, `_cage`. For example: `Table01_low.fbx`, `Table01_high.fbx`, `Table01_cage.fbx`.

---

### **Mesh Binaries Publication**

| Data | Purpose     | .blend | .fbx | .gltf | .obj | Notes |
| ---- | ----------- | :----: | :--: | :---: | :--: | ----- |
| Mesh | Baking      |        |  ✓   |       |      |       |
| Mesh | Publication |   ✓    |  ✓   |   ✓   |  ✓   |       |

---

### **a. Image Binaries Publication**

Texture files follow this naming pattern:

```
T_ObjectName##_TextureType.extension
```

**Example:** `T_Table01_BaseColor.webp`

| Texture Type       | Purpose     | .png | .jpg | .webp | Notes                              |
| ------------------ | ----------- | :--: | :--: | :---: | ---------------------------------- |
| Reference          | Reference   |      |  ✓   |       | Any format, optional               |
| BaseColor          | Publication |      |      |   ✓   | 8-bit sRGB                         |
| Normal             | Publication |  ✓   |      |       | 16-bit linear, OpenGL              |
| Roughness          | Publication |      |      |   ✓   | 8-bit linear                       |
| Metalness          | Publication |      |      |   ✓   | 8-bit linear                       |
| ORM                | Publication |      |      |   ✓   | 8-bit linear, packed channels      |
| MetallicSmoothness | Publication |      |      |   ✓   | 8-bit                              |
| Opacity            | Publication |      |      |   ✓   | 8-bit linear, optional             |
| Emissive           | Publication |      |      |   ✓   | 8-bit sRGB, optional               |

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

## **10. Architecture Projects**

When we explicitly refer to *"Architecture"*, we mean the professional discipline of building design, rather than the concept of "architecture" as used in computer science.

For architectural projects, naming conventions should prioritize human readability over strict technical formatting. Binary files and related data should follow the structure:

```
Noun Adjective Extra-Adjective
```

Every first letter of each word should be capitalized. If the adjective or extra-adjective contains multiple words, it must use a hyphen (`-`) as punctuation. The adjectives should be ordered from the most general category to the more specific.

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

- [Rust API Guidelines for Naming](https://rust-lang.github.io/api-guidelines/naming.html)
- [Rust Naming Conventions](https://github.com/rust-lang/rfcs/blob/master/text/0430-finalizing-naming-conventions.md)
- [Unreal Engine Recommended Asset Naming Convention](https://dev.epicgames.com/documentation/en-us/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects)
- [Best practices for organizing your Unity project](https://unity.com/how-to/organizing-your-project)
