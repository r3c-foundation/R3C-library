## 📘 **README.md — R3C-library (Build with No LLVM)**


# 🧱 R3C-library — Build with No LLVM
> “No LLVM. No Middle Layer. Just Metal.” ⚙️  
> The core runtime and standard library bridge for the LLVM-free Rust ecosystem.

---

## 🧭 Overview

**R3C-library** is the foundational runtime and standard support layer  
for the [R3C ecosystem](https://github.com/r3c-foundation/r3c).

It provides **LLVM-free system bindings, I/O, math, and memory management**,  
enabling **Rust and C++** to compile and execute **without depending on LLVM**.

```

C / C++ / Rust  →  R3C-library  →  NASM / ASM  →  Hardware

````

---

## ⚙️ Architecture

```text
┌──────────────────────────────┐
│          User Code           │
│  (C++ / Rust / Assembly)     │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│         R3C-library          │
│  Core: r3c_std, r3c_sys,     │
│        r3c_io, r3c_math      │
│  Role: LLVM-independent      │
│        build/runtime bridge  │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│           R3C Core           │
│  (C++ → Rust → NASM)         │
│  Self-hosting compiler core  │
└──────────────┬───────────────┘
               │
               ▼
        🧱 Direct Assembly (NASM)
````

---

## 🧰 Core Modules

| Module        | Description                                | Notes                          |
| ------------- | ------------------------------------------ | ------------------------------ |
| `r3c_std`     | Minimal standard library replacement       | no_std ready                   |
| `r3c_sys`     | OS abstraction (POSIX, Windows, baremetal) | Embedded compatible            |
| `r3c_io`      | File / stream / serial I/O                 | Safe wrappers for system calls |
| `r3c_math`    | Fixed / floating point routines            | ASM optimized                  |
| `r3c_alloc`   | Memory manager                             | Replaces LLVM allocator        |
| `r3c_startup` | Entry point / startup bridge               | ASM → Rust bootstrap           |

---

## 🔧 Build (CMake)

```bash
git clone https://github.com/r3c-foundation/r3c-library
cd r3c-library
cmake -B build -S .
cmake --build build --parallel 4
```

**CMakeLists.txt:**

```cmake
cmake_minimum_required(VERSION 3.22)
project(r3c_library LANGUAGES C CXX ASM)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_BUILD_TYPE Release)

add_library(r3c_library STATIC
    src/r3c_std.cpp
    src/r3c_sys.cpp
    src/r3c_io.cpp
    src/r3c_math.cpp
    src/r3c_alloc.cpp
    src/r3c_startup.s
)

target_include_directories(r3c_library PUBLIC include)
target_compile_definitions(r3c_library PUBLIC R3C_NO_LLVM=1)
```

---

## 🦀 Rust Integration

**Cargo.toml**

```toml
[dependencies]
r3c_library = { path = "../r3c-library" }
```

**main.rs**

```rust
fn main() {
    unsafe { r3c_library::init_runtime(); }
    println!("🦀 Running Rust with no LLVM backend!");
}
```

---

## 🧱 Philosophy

> LLVM built the bridge between languages —
> **R3C-library removes the bridge, and lets Rust walk on its own.**

* Designed for **industrial and embedded** Rust builds
* Fully compatible with **R3C Core** and **HalRust**
* A foundation for the **Rust-LTSS (Long-Term Sustain System)** vision

---

## 🧩 Ecosystem

| Project                                                  | Role               | Description                              |
| -------------------------------------------------------- | ------------------ | ---------------------------------------- |
| [R3C Core](https://github.com/r3c-foundation/r3c)        | Compiler           | LLVM-free transpiler (C++ → Rust → ASM)  |
| [HalRust](https://github.com/r3c-foundation/HalRust)     | Transitional Layer | Hybrid compiler for LLVM ↔ ASM           |
| [Rust-LTSS](https://github.com/r3c-foundation/Rust-ltss) | LTS Rust Ecosystem | Industrial long-term stable Rust         |
| [cpppm](https://github.com/r3c-foundation/cpppm)         | Build Tool         | Lightweight C++ package manager          |
| **R3C-library**                                          | **Runtime Layer**  | **LLVM-free standard library & runtime** |

---

## 📜 License

**MIT License**
Free to modify, use, and redistribute — even without LLVM.

---

## 🪶 Author’s Note

R3C-library is not a replacement, but a restoration —
of Rust’s ability to exist without external control.

> “Dependence is comfort.
> Independence is evolution.
> Coexistence is transition.”

— R3C Foundation (2025)


---


