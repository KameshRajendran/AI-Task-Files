# Task List

## Task ID: TASK-001 - Implement Export Support for Redaction Annotations

### Summary
Add support for exporting redaction annotations in the PDF Viewer component to ensure these annotations can be properly saved and later imported.

### Context
Currently, the PDF Viewer supports exporting various annotation types (shape, ink, text markup, etc.), but lacks support for redaction annotations. Redaction annotations are used to permanently remove sensitive content from PDFs and have unique properties that need to be preserved during export. The export functionality is crucial for allowing users to save their redaction work and share it across different instances of the application.

### Requirements
- Add `PdfLoadedAnnotationType.RedactionAnnotation` to the supported annotation types list in the `ExportAnnotation` method
- Implement export handling for redaction-specific properties:
  - MarkerOutlineOpacity
  - MarkerFillColor
  - MarkerBorderColor
  - OverlayText
  - RepeatText
- Ensure compatibility with both JSON and XFDF export formats
- Follow the existing pattern used for rectangle annotations while accounting for redaction-specific properties
- Integrate with the existing `m_renderer.RedactionAnnotationList` that's already used in the import process

### Acceptance Criteria
- [ ] The `ExportAnnotation` method in PdfRenderer.cs includes `PdfLoadedAnnotationType.RedactionAnnotation` in its `needAnnotationTypes` list
- [ ] All redaction-specific properties (MarkerOutlineOpacity, MarkerFillColor, MarkerBorderColor, OverlayText, RepeatText) are correctly included in the exported data
- [ ] Redaction annotations can be successfully exported in both JSON and XFDF formats
- [ ] Exported redaction annotations can be successfully re-imported using the existing import functionality
- [ ] The solution maintains backward compatibility with existing annotation export/import features
- [ ] Unit tests verify the successful export of redaction annotations with various property combinations
- [ ] Code follows the project's style guidelines and includes appropriate documentation