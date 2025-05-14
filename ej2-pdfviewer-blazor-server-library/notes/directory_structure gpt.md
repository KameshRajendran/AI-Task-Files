# Syncfusion.SfPdfViewer.AspNet.Core Solution Structure

Based on the provided code files, here's the structure of the Syncfusion PDF Viewer for Blazor project:

## Root Directory
├── LICENSE.txt
├── AspNet.Core_README.md
├── NuGet.config
├── packageNETStandard.bat
├── remove-existing-package.bat
├── copyNETStandard.bat
├── Syncfusion.SfPdfViewer.AspNet.Core.sln

## Source Code (Src)
├── Src/
│   ├── AssemblyInfo.cs
│   ├── Syncfusion.SfPdfViewer.AspNet.Core.csproj
│   ├── Syncfusion.SfPdfViewer.AspNet.Core.nuspec
│   ├── syncfusion_logo.png
│   ├── LICENSE.txt
│   ├── AspNet.Core_README.md
│   ├── Implementation/
│   │   ├── SfPdfViewer/
│   │   │   ├── FormFields.cs
│   │   │   ├── PageRenderer.cs
│   │   │   ├── PdfRenderer.cs
│   │   │   └── [Other implementation files]

## Core Components
├── CacheManagement.cs
├── DrawImagePixels.cs
├── ImageStructure.cs
├── Signature.cs
├── PdfiumNative.cs
├── SfBookmark.cs

## Notes Directory
├── notes/
│   ├── project_overview.md
│   ├── project_overview_claude.md
│   ├── project_overview_deep.md
│   └── project_overview_gpt.md

## Key Component Structure:

### 1. Rendering Components
- **PageRenderer**: Handles PDF page visualization
  - `UpdatePageText()`, `RemoveExtraSpace()`, `MinORMaxValue()`, `FindImagedata()`
  - Contains `StampAnnotation` and `TextMarkupAnnotation` classes

### 2. Cache Management
- **CacheManagement**: Handles document caching
  - `SetDocumentInfo()`, `GetDocument()`, `ConvertBytesToObject<T>()`
  - Uses both `IMemoryCache` and `IDistributedCache`

### 3. Image Processing
- **DrawImagePixels**: Bit-level image operations
- **ImageStructure**: `EstimateBit()` for image analysis

### 4. Document Structure
- **SfBookmark**: Bookmark handling for document navigation
  - `SfBookmarkDestination`, `BookmarkStyles` classes

### 5. Annotations
- **Signature**: Handles digital signatures
  - `getSignatureBounds()`, `saveSignatureAsAnnotatation()`

### 6. Platform Interop
- **PdfiumNative**: Contains native PDF structure definitions
  - `RECTF` for rectangle coordinates

## Project Configuration:
- Multi-targeting .NET 8.0 and .NET 9.0
- Multiple build configurations (Debug, Release, Release-Xml)
- Dependency on SkiaSharp for cross-platform rendering
- NuGet package configuration in Syncfusion.SfPdfViewer.AspNet.Core.nuspec

## Build Process:
- Script files for package generation and deployment
- Custom XML documentation generation in Release-Xml mode

This project is licensed under the Syncfusion Software License Agreement (Enterprise Edition), as specified in LICENSE.txt.