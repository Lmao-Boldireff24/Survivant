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
## 🛠️ Third-Party Libraries

This section contains a list of the libraries used in this project, along with a brief explanation of their roles and importance.

<details>
<summary>1. GLFW (Windowing)</summary>

#### Use
This open-source library is crucial for creating and managing windows compatible with multiple rendering APIs. GLFW provides high level multi-platform abstraction for graphical applications.

#### Justification
GLFW is the standard windowing API for OpenGL. The team already has experience with it.

#### Loading process
1. Initialise with [glfwInit()](https://www.glfw.org/docs/3.3/group__init.html#ga317aac130a235ab08c6db0834907d85e)
2. Set OpenGL version (4.6) with `glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4)` & `glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 6)`
3. Create a default window with `glfwCreateWindow(width, height, title)`
4. Select the window with `glfwMakeContextCurrent(window)`
5. Use GLFW in the main loop
6. Destroy the window with `glfwDestroyWindow(window)` & `glfwTerminate()`

#### Source files integration
Added using _FetchContent_

#### Type of library
Static

#### Sources
- [GLFW Documentation](https://www.glfw.org/docs/3.3/quick.html)

</details>

<details>
<summary>2. Glad (GL Loader-Generator)</summary>

#### Use
Glad loads graphics API functions. Supports OpenGL, Vulkan, WGL, EGL, GLX, and OpenGL ES.

#### Justification
Glad is standard, easy to use, integrates with GLFW, and familiar to the team.

#### Loading process
1. Initialize the graphics API with `gladLoadGL(glfwGetProcAddress)`
2. Use OpenGL functions normally

#### Source files integration
Source code versioned with the project

#### Type of library
Static

#### Sources
- [Glad GitHub](https://github.com/Dav1dde/glad/wiki/C#quick-start)
- [Glad Generator](https://gen.glad.sh/)

</details>

<details>
<summary>3. ImGui (User Interface)</summary>

#### Use
Graphical user interface library for engine development.

#### Justification
Lightweight, easy-to-use, few dependencies, prioritizes iteration speed.

#### Loading process
1. `IMGUI_CHECKVERSION()` & `ImGui::CreateContext()`
2. Set input flags: `io.ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard`
3. Initialize backend: `ImGui_ImplGlfw_InitForOpenGL()` & `ImGui_ImplOpenGL3_Init()`
4. In main loop: `ImGui_ImplOpenGL3_NewFrame()`, `ImGui_ImplGlfw_NewFrame()`, `ImGui::NewFrame()`
5. Render frame: `ImGui::Render()`, `ImGui_ImplOpenGL3_RenderDrawData(ImGui::GetDrawData())`
6. Shutdown: `ImGui_ImplOpenGL3_Shutdown()`, `ImGui_ImplGlfw_Shutdown()`, `ImGui::DestroyContext()`

#### Source files integration
Added using _FetchContent_

#### Type of library
Static

#### Sources
- [ImGui GitHub](https://github.com/ocornut/imgui#dear-imgui)
- [Getting Started](https://github.com/ocornut/imgui/wiki/Getting-Started)

</details>

<details>
<summary>4. PhysX (Physics)</summary>

#### Use
Simulates 3D physics: actors, collisions, joints, rigid bodies, raycasts.

#### Justification
Powerful, GPU-accelerated optional, industry-standard for realistic physics.

#### Loading process
1. `PxCreateFoundation(PX_PHYSICS_VERSION, callback, error_callback)`
2. Connect foundation to PVD socket (`PxCreatePvd`, `PxDefaultPvdSocketTransportCreate`, `mPvd->connect(...)`)
3. Create physics object (`PxCreatePhysics(...)`)
4. Use physics in main loop
5. Release objects: `mPhysics->release()`, `mFoundation->release()`

#### Source files integration
***TODO***

#### Type of library
***TODO***

#### Sources
- [PhysX Overview](https://gameworksdocs.nvidia.com/PhysX/4.0/documentation/PhysXGuide/Manual/Introduction.html#a-brief-overview-of-physx)
- [Foundation API](https://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/apireference/files/group__foundation.html)

</details>

<details>
<summary>5. SoLoud (Audio)</summary>

#### Use
Audio engine for playing sounds and music.

#### Justification
Simple API, supports multiple formats, filters, queues, loops, volume, and speed control.

#### Loading process
1. `SoLoud::Soloud gSoloud; gSoloud.init()`
2. Load sound: `SoLoud::Wav gWave; gWave.load("pew_pew.wav")`
3. Play: `gSoloud.play(gWave)`
4. Shutdown: `gSoloud.deinit()`

#### Source files integration
Added using _FetchContent_

#### Type of library
Static

#### Sources
- [SoLoud](https://solhsa.com/soloud/index.html)

</details>

<details>
<summary>6. Assimp (3D Model Importer)</summary>

#### Use
Load and process 3D models in multiple formats.

#### Justification
Flexible, supports many formats, simplifies model integration.

#### Loading process
1. Create importer: `Assimp::Importer importer`
2. Load model: `importer.ReadFile("model.obj", aiProcess_Triangulate | aiProcess_JoinIdenticalVertices | aiProcess_SortByType)`
3. Process data from scene

#### Source files integration
Added using _FetchContent_

#### Type of library
Static

#### Sources
- [Assimp Official](https://www.assimp.org/)
- [GitHub](https://github.com/assimp/assimp)
- [Docs](https://assimp-docs.readthedocs.io/en/latest/usage/use_the_lib.html)

</details>

<details>
<summary>7. STB image (Texture Importer)</summary>

#### Use
Lightweight library for loading images in multiple formats.

#### Justification
Fast and easy solution for textures.

#### Loading process
1. Load: `stbi_load("texture.png", &width, &height, &channels, 0)`
2. Free memory: `stbi_image_free()`

#### Source files integration
Versioned with project

#### Type of library
Static – Header Only

#### Sources
- [STB Image](https://github.com/nothings/stb/blob/master/stb_image.h)

</details>


---

## Conclusion

Survivant is a modular, cross-platform C++ game engine showcasing:

- Real-time rendering and editor functionality  
- Modular engine design  
- Third-party library integration (GLFW, Glad, ImGui, PhysX, SoLoud, Assimp, STB)  
- Component-based architecture and asset pipeline

> TODO: Add final notes on performance, limitations, future work, and contribution guidelines


