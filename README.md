# Survivant
## C++ Game Engine
 
C++ • OpenGL 4.6 • PhysX • ImGui

---

## Overview

Survivant is a modular C++ game engine developed as a team project, with a focus on **engine architecture**, **real-time rendering**, **tooling**, and **systems integration**.

Rather than targeting end-users or production deployment, Survivant is designed as a **technical showcase** demonstrating low-level engine development skills and design decisions.

> ⚠️ This project is not intended to be a consumer-ready engine.  
> Its purpose is educational, architectural, and portfolio-oriented.

---

## 📸 Screenshots & Media

> TODO: Add editor screenshots  
> TODO: Add runtime rendering screenshots  
> TODO: Add short GIFs demonstrating features (editor, physics, rendering)

docs/images/editor.png
docs/images/runtime.png
docs/images/physics.gif

---

## 🚀 Features

- Modular C++ engine architecture
- OpenGL 4.6 rendering backend
- Real-time editor built with ImGui
- Runtime / editor separation
- PhysX-based 3D physics simulation
- Asset pipeline for models, textures, shaders
- Audio playback using SoLoud
- Component-based scene structure
- Cross-platform windowing and input (GLFW)

> TODO: Add details about ECS vs custom component model  
> TODO: Clarify scripting system scope (language, bindings, limitations)

---

## 🧭 Project Scope

Survivant is a **learning-focused engine project** aimed at exploring:

- Rendering abstraction
- Engine modularity
- Third-party library integration
- Editor tooling
- Runtime architecture

It is **not** intended to:
- Replace commercial engines
- Provide production-ready workflows
- Offer end-user installation support

---

## 🏗️ Engine Architecture

### Folder Structure

## 🏗️ Engine Architecture

### Folder Structure

Dependencies
Resources
├── Editor
│   ├── Fonts
│   ├── Models
│   └── Scripts
└── Engine
    ├── Fonts
    ├── Materials
    ├── Models
    ├── Scripts
    └── Shaders

Source
├── Editor+
├── Runtime+
├── Engine
│   ├── App+
│   ├── Audio+
│   ├── Core+
│   ├── Physics+
│   ├── Rendering+
│   ├── Scripting+
│   ├── UI+
│   └── Test+

---

## Graphics API

For this project, we use **OpenGL 4.6** for its relative ease of use, long industry history, and wide hardware compatibility. The team's familiarity with OpenGL allows us to focus more on engine architecture, tooling, and systems integration rather than low-level graphics issues.

> TODO: Add notes about rendering abstraction and pipeline management  
> TODO: Include info about shader management and material system  

---

## Third-Party Libraries

This section lists the key libraries used in Survivant, with explanations of their role, integration, and usage.

- Explanation of how it works  
- Justification for choosing the library  
- Library loading process  
- Integration of source files (precompiled .lib/.dll, FetchContent, or header-only)  
- Static (.lib) / dynamic (.dll) library  

---

### 1. GLFW (Windowing)

#### Use
GLFW provides multi-platform window creation, input handling, and OpenGL context management.

#### Justification
GLFW is the standard windowing library for OpenGL projects and fits our cross-platform goals.

#### Loading process
1. Initialize with `glfwInit()`.  
2. Set OpenGL version with `glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4)` and `glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 6)`.  
3. Create a window with `glfwCreateWindow(width, height, title)`.  
4. Make the context current with `glfwMakeContextCurrent(window)`.  
5. Destroy the window and terminate with `glfwDestroyWindow(window)` and `glfwTerminate()`.

#### Source files integration
Added using _FetchContent_

#### Type
Static

#### Sources
- [GLFW Documentation](https://www.glfw.org/docs/3.3/quick.html)

---

### 2. Glad (GL Loader)

#### Use
Loads OpenGL functions dynamically for the specified API version.

#### Justification
Industry-standard, works seamlessly with GLFW, minimal overhead.

#### Loading process
1. Initialize OpenGL functions using `gladLoadGL(glfwGetProcAddress)`.  
2. Use OpenGL API functions normally.

#### Source files integration
Versioned with the project

#### Type
Static

#### Sources
- [Glad Quick Start](https://github.com/Dav1dde/glad/wiki/C#quick-start)  
- [Glad Generator](https://gen.glad.sh/)

---

### 3. ImGui (User Interface)

#### Use
Immediate-mode GUI library for creating the editor and debugging tools.

#### Justification
Lightweight, minimal dependencies, fast iteration, integrates easily with OpenGL + GLFW.

#### Loading process
1. Initialize context with `IMGUI_CHECKVERSION()` and `ImGui::CreateContext()`.  
2. Configure input flags: `io.ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard`.  
3. Initialize backends: `ImGui_ImplGlfw_InitForOpenGL()` and `ImGui_ImplOpenGL3_Init()`.  
4. Start a frame with `ImGui_ImplOpenGL3_NewFrame()`, `ImGui_ImplGlfw_NewFrame()`, `ImGui::NewFrame()`.  
5. Render frame with `ImGui::Render()` and `ImGui_ImplOpenGL3_RenderDrawData(ImGui::GetDrawData())`.  
6. Shutdown: `ImGui_ImplOpenGL3_Shutdown()`, `ImGui_ImplGlfw_Shutdown()`, `ImGui::DestroyContext()`.

#### Source files integration
Added using _FetchContent_

#### Type
Static

#### Sources
- [ImGui GitHub](https://github.com/ocornut/imgui#dear-imgui)  
- [Getting Started](https://github.com/ocornut/imgui/wiki/Getting-Started)

---

### 4. PhysX (Physics)

#### Use
3D physics simulation including rigid bodies, collisions, joints, and raycasts.

#### Justification
Powerful low-level physics engine with GPU acceleration (CUDA) for demanding simulations.

#### Loading process
1. Initialize foundation: `PxCreateFoundation()`.  
2. Connect PVD for debug visualization: `PxCreatePvd()` + transport + `mPvd->connect()`.  
3. Create physics object: `PxCreatePhysics()`.  
4. Use callbacks in main loop.  
5. Release objects: `mPhysics->release()`, `mFoundation->release()`.

#### Source files integration
***TODO***

#### Type
***TODO***

#### Sources
- [PhysX Overview](https://gameworksdocs.nvidia.com/PhysX/4.0/documentation/PhysXGuide/Manual/Introduction.html#a-brief-overview-of-physx)  
- [Foundation API](https://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/apireference/files/group__foundation.html)

---

### 5. SoLoud (Audio)

#### Use
Audio engine for playback, filtering, volume control, and multi-format support.

#### Justification
Simple to integrate, supports looping and multiple audio formats.

#### Loading process
1. Initialize engine: `SoLoud::Soloud gSoloud; gSoloud.init()`.  
2. Load audio: `SoLoud::Wav gWave; gWave.load("file.wav")`.  
3. Play: `gSoloud.play(gWave)`.  
4. Shutdown: `gSoloud.deinit()`.

#### Source files integration
Added using _FetchContent_

#### Type
Static

#### Sources
- [SoLoud Homepage](https://solhsa.com/soloud/index.html)

---

### 6. Assimp (3D Model Importer)

#### Use
Load and process 3D models from multiple formats (OBJ, FBX, etc.).

#### Justification
Flexible and robust model loader with wide format support.

#### Loading process
1. Create importer: `Assimp::Importer importer`.  
2. Load model: `importer.ReadFile("model.obj", flags)`.  
3. Process the scene data.

#### Source files integration
Added using _FetchContent_

#### Type
Static

#### Sources
- [Assimp](https://www.assimp.org/)  
- [GitHub](https://github.com/assimp/assimp)  
- [Docs](https://assimp-docs.readthedocs.io/en/latest/usage/use_the_lib.html)

---

### 7. STB image (Texture Importer)

#### Use
Header-only library for loading images in multiple formats.

#### Justification
Lightweight, fast, and easy to integrate for textures.

#### Loading process
1. Load images with `stbi_load()`.  
2. Free memory with `stbi_image_free()` when done.

#### Source files integration
Versioned with the project

#### Type
Static – Header Only

#### Sources
- [STB Image](https://github.com/nothings/stb/blob/master/stb_image.h)

---

## Conclusion

Survivant is a modular, cross-platform C++ game engine showcasing:

- Real-time rendering and editor functionality  
- Modular engine design  
- Third-party library integration (GLFW, Glad, ImGui, PhysX, SoLoud, Assimp, STB)  
- Component-based architecture and asset pipeline

> TODO: Add final notes on performance, limitations, future work, and contribution guidelines


