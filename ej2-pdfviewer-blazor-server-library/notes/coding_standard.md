
# 📘 Syncfusion.SfPdfViewer.AspNet.Core –  Code Generation Standards

## 🔐 Objective
Ensure that code adheres to **production-grade quality**, with strong emphasis on:
- **Performance and memory optimization**
- **Null safety**
- **Clean, modular architecture**
- **Maintainability and readability**

---

## 📏 General Class Guidelines

| Rule | Description |
|------|-------------|
| **CL001** | A single class file **must not exceed 500 lines** of code. |
| **CL002** | No nested or inner classes. Each class must reside in its **own `.cs` file**. |
| **CL003** | Use **`partial`** classes if the implementation is large and logically separable. |
| **CL004** | Each class must have a single, clearly defined **responsibility** (SRP – Single Responsibility Principle). |
| **CL005** | Avoid "God objects" – classes should not manage unrelated functionalities. |

---

## 🔄 Method Design

| Rule | Description |
|------|-------------|
| **MD001** | Each method should have **≤ 40 lines** of code. |
| **MD002** | Prefer **pure functions** where applicable (no side effects). |
| **MD003** | Avoid deep nesting (`if`/`else`/`switch`) – refactor into smaller methods. |
| **MD004** | Use meaningful names (`ParsePdfStream()` instead of `DoIt()` or `Process1()`). |
| **MD005** | Public methods must include **XML documentation comments**. |
| **MD006** | Avoid method overloading beyond 3 overloads – use option objects if needed. |

---

## 🧠 Memory & Performance

| Rule | Description |
|------|-------------|
| **PM001** | Avoid unnecessary heap allocations. |
| **PM002** | Avoid closures and lambdas in performance-critical paths. |
| **PM003** | Cache expensive computations or lookups. |

---

## ⚠️ Null & Exception Handling

| Rule | Description |
|------|-------------|
| **EX001** | All code must be **null-safe**. Use **null guards** or `ArgumentNullException.ThrowIfNull`. |
| **EX002** | Prefer `TryGet`, `TryParse` patterns over exceptions for control flow. |
| **EX003** | Never catch generic `Exception` unless absolutely necessary. |
| **EX004** | Always log exceptions if caught. Do not silently ignore. |
| **EX005** | Use `Nullable Reference Types` with `#nullable enable` enabled in all files. |

---

## 🧱 Architecture & Dependency Rules

| Rule | Description |
|------|-------------|
| **AR001** | Use **interface-based** programming for public APIs and internal modules. |
| **AR002** | Follow **dependency inversion**; high-level modules should not depend on low-level modules directly. |
| **AR003** | Avoid static state except for constants or read-only caches. |
| **AR004** | Avoid circular dependencies – enforce clear layer boundaries. |


---

## 📂 File & Naming Conventions

| Rule | Description |
|------|-------------|
| **NC001** | All file names must match their class names (e.g., `PdfRenderer.cs`). |
| **NC002** | Use **PascalCase** for class names and **camelCase** for fields. |
| **NC003** | Constants must be in UPPER_CASE_WITH_UNDERSCORES. |
| **NC004** | Prefix private fields with `_` (e.g., `_streamReader`). |

---