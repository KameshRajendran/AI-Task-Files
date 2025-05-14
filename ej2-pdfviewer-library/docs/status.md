
## Task ID: TASK-010 - Implement Redaction Annotation Editing Support

**Status:** IMPLEMENTATION_COMPLETE

### Summary
Extend the `editAnnotation` method in annotation-editor.ts to support programmatic editing of redaction annotation properties, following the same pattern used for other annotation types.

### Context
The PDF viewer currently supports rendering redaction annotations and adding them through both programmatic methods and user interactions. The `editAnnotation` method handles property changes for various annotation types (text markup, sticky notes, shapes, etc.) but lacks implementation for redaction annotations. We need to add this functionality to ensure complete support for redaction annotations.

### Implementation Details
- Enhanced the redaction case in the `editAnnotation` method to properly handle all redaction-specific properties
- Implemented property change detection for all redaction annotation properties:
  - markerFillColor
  - markerBorderColor
  - markerOpacity
  - overlayText
  - repeatText
  - fontColor
  - fontSize
  - fontFamily
  - textAlign
  - bounds
  - isPrint
  - isLock
- Added proper undo/redo support by updating the redoClonedObject
- Ensured proper event triggering for UI updates
- Added integration with the redactionOverlayText module to refresh overlay text when properties change
- Maintained consistency with the existing pattern used for other annotation types

### Testing
The implementation has been tested to ensure:
- All redaction annotation properties can be edited programmatically
- Changes to redaction properties are correctly applied to the annotation object
- Visual representation of the redaction annotation updates to reflect the changes
- Changes are properly stored in the annotation collections

## Task ID: TASK-008 - Redaction Annotation Export Support

### Summary
Extend the PDF Viewer's annotation export system to support redaction annotations, allowing users to export and import redaction annotations along with other annotation types.

### Context
The PDF Viewer component currently supports exporting various annotation types such as shapes, text markup, and sticky notes. Redaction annotation support has been added to the viewer, but the export mechanism needs to be updated to include these annotations when users export their documents.

### Requirements
1. Support exporting redaction annotations in both JSON and XFDF formats
2. Maintain consistency with the existing annotation export system
3. Ensure proper handling of redaction-specific properties
4. Allow export of redaction annotations individually or with other annotation types

### Acceptance Criteria
- The `constructJsonDownload` method includes redaction annotations in its output JSON
- Implement proper usage of the existing `saveRedactionAnnotations()` method
- Add a helper method `isRedactionAnnotationModule()` to check for the redaction module
- Include redaction annotations in the `isAnnotationsExist` check
- Ensure proper date formatting for redaction annotations using `updateModifiedDateToLocalDate`
- Update annotation collections after export via `updateDocumentAnnotationCollections()`
- Please refer the `saveShapeAnnotations` from `shape-annotation-export-import.ts` file for reference.
- The exported JSON structure for redaction annotations follows the same pattern as other annotation types
- Redaction annotations correctly include all required properties:
   - Basic properties (AnnotName, AnnotType, Author, Bounds)
   - Redaction-specific properties (MarkerFillColor, MarkerBorderColor, OverlayText, FontSize, etc.)
- Support for both JSON and XFDF formats when exporting redaction annotations
- Exported redaction annotations can be successfully imported back

## Task ID: TASK-007
### Summary: Add Annotation Programmatically
**Status:** IMPLEMENTATION_COMPLETE
### Summary
We have already read the redaction annotation from the server side and rendered in the client side (tasks 001 to 005). We have also provided support to draw it interactively. Now we need to provide support to add the redaction annotation programmatically.
### Context
We have already read the redaction annotation from the server side and rendered in the client side (tasks 001 to 005). We have also provided support to draw it interactively. Now we need to provide support to add the redaction annotation programmatically.
### Requirements
- Refer to the `createAnnotation` method in the `annotation-handler.ts` file, which receives the annotation from the server side and adds the annotation programmatically.
- Refer to the `createRequestForImportAnnotations` inside the `importAnnotations` method to see how shape annotations are handled.
- Implement similar logic for redaction annotation.
- Redaction annotation is similar to rectangle annotation. So all the properties like rectangle annotation have to be handled for redaction annotation as well.
- Please thoroughly review the createRequestForImportAnnotations method and its internal method calls to implement the addition of redaction annotations programmatically

### Acceptance Criteria

### Rendering
- [ ] Redaction annotation renders correctly on the PDF page
- [ ] Rendering is consistent across zoom levels

### User Interaction
- [ ] Drag operations function smoothly
- [ ] Resize handles appear correctly
- [ ] Resize operations maintain proper functionality
- [ ] Selection indicators appear appropriately

### Expected Result
The system should allow developers to programmatically add redaction annotations with all the necessary properties, similar to how rectangle annotations are currently handled.



## Task ID: TASK-006 - Overlay Text support for Redaction Annotation
### Summary
We have already implemented hover color changes for redaction annotation. Now we have to to provide the Overlay text support for the redaction annotation similar to hover color. 
**Status:** NEEDS_PLAN
We have already implemented hover color changes for redaction annotation. Now we have to to provide the Overlay text support for the redaction annotation similar to hover color. 
#### Requirements
- Please check the `drawing.ts` ,`pdf-annotation.ts`,`pdfviewer-base.ts`,`redaction-annotation.ts` files in the `old_Implementation` flder which is old file of our source. 
- Please check the `renderRedactionOverlayText` method and how it is used in this place
- Please analysis when the Overlay text is visibile. 
- Please analysis the `RepeatText` functionlity implemented.
- When mouse over the redaction annotation the overlay text should show similar to redaction annotation mouse over color change
- When mouse over the redaction annotation the overlay text will be hidden similar to redaction annotation mouse over color change
- Over lay text should work on the below (Left, right and center alignments)
- It should work have capability to wrap both text and word wrap
- The text should not visible beyond the shapes 4 borders.


## Task ID: TASK-005 - Redaction annotation Hover color change while mouse over
### Summary
Implement hover-based background color change for redaction annotations in the Blazor PDF Viewer component.

**Status:** IMPLEMENTATION_COMPLETE
#### Context
We have already have the support to render the redaction annotation programatically and load from document load. Now we have to provide the Hover color change to redaction annotation.

#### Requirements
- Examine the `setCursor` method located in the `@pdfviewer-base.ts` file to understand how cursor changes are handled during hover events.
- Analyze the usage and role of the `renderDrawing` method in `@sf-pdfviewer2-fn.ts` and its connection to annotation rendering.
- Understand the overall flow of cursor behavior and annotation re-rendering during user interactions.
- Now, Provide the support to change the color of the redaction annotation in the `setCursor` method located in the `@pdfviewer-base.ts`.
- Once the mouse leave from the redaction annotation, the redaction annotation color have to reset to previous color.  

 #### Acceptance Criteria:
1. On hovering over a redaction annotation, its background color changes to yellow.
2. On mouse leave, the background color reverts to its original state.
3. Other annotations remain unaffected by this behavior.
4. Implementation uses existing rendering and cursor logic where appropriate.
4. Code is modular, maintainable, and aligns with current coding standards used in the PDF Viewer codebase.

## Task ID: TASK-004 - Interactive Redaction Annotation Drawing
### Summary
We have already rendered the redaction annotation on the client side. We have now going to provide the support to render the redaction annotation programatically. 

**Status:** IMPLEMENTATION_COMPLETE

#### Context
Redaction annotations are already rendered in the PDF Viewer when they exist in the data. However, users currently cannot draw redaction annotations interactively. This task aims to implement the missing functionality for interactive redaction annotation creation.

#### Requirements
- Analysis the `setAnnotationMode` function in `@annotation.ts` to support the activation of redaction annotation drawing mode. 

- Analysis `this.shapeAnnotationModule.setAnnotationType` called from `annotation.ts` and implement the similar logis for Redaction Annotation Also. 

- Verify the `updateShapeProperties` method in the `shape-annotation.ts` and update the similar logic to redaction annotation also.

- Ensure `viewerContainerOnMousedown`, `viewerContainerOnMousemove`, and `viewerContainerOnMouseup` in `@pdfviewer-base.ts` handle redaction annotation drawing, following the rectangle annotation pattern.  
- Apply redaction-specific properties and rendering logic during the drawing process.

### Implementation Guidelines  

- Modify the `setAnnotationMode` function in `@annotation.ts` to accept `'Redaction'` as a valid mode to initiate redaction annotation drawing.  
- Once `setAnnotationMode('Redaction')` is triggered, user interactions (mouse down, move, up) should initiate redaction drawing, similar to how rectangles are handled.  
- These drawing interactions are controlled in `viewerContainerOnMousemove`, `viewerContainerOnMousedown`, and `viewerContainerOnMouseup` within `@pdfviewer-base.ts`.  
- Implement the rendering logic for redaction annotations inside `redaction-annotation.ts`.  


#### Expected Result
- When `setAnnotationMode('Redaction')` is activated, users can click and drag on the PDF Viewer to draw a redaction annotation interactively, just like a rectangle annotation.




## Task ID: TASK-003
### Summary: Implement the redaction specific implementation in missed places.
**Status:** IMPLEMENTATION_INPROGRESS

#### Context
The redaction annotation implementation was completed in TASK-002, which rendered redaction annotations on the PDF viewer. However, we've identified several places in the codebase where redaction-specific implementations were missed and need to be added to ensure complete functionality.

#### Requirements
1. Already Updated manually for the following methods to handle redaction annotations properly:
   - `onPageRender` in `pdfviewer-base-rendering.ts`
   - `renderAnnotations` in `pdfviewer-base-rendering.ts`
   - `renderAnnotationCollections` in `sticky-notes-renderer.ts`
2. i can see some places missed `updateAnnotationsInDocumentCollections` method in `sticky-notes-storage.ts`
2. Review and update the `drawing.ts` file to include redaction annotation handling.

3. Perform a comprehensive codebase review to identify any other locations where redaction annotations should be handled but might be missing.

4. Ensure that all implementation follows the same pattern as rectangle annotations but with redaction-specific features.

5. Verify that redaction annotations render correctly with proper styling attributes (markerFillColor, markerBorderColor, markerOutlineOpacity) at all zoom levels and page positions.

6. If any code changes are needed in an existing code file, please suggest only the necessary code modifications. Avoid recreating the entire file. 

7. Implement the changes in the `redaction-annotation.ts` file 

#### Acceptance Criteria:
1. All methods identified in the requirements properly handle redaction annotations.
2. The `drawing.ts` file is updated to support redaction annotations.
3. All potential locations in the codebase that should handle redaction annotations have been reviewed and updated as needed.
4. Redaction annotations display and function correctly throughout the application.
5. No regressions in existing functionality for other annotation types.
6. Code follows the established patterns for annotation handling in the codebase.
 

## Task ID: TASK-002
### Summary: Render the Redaction Annotation
**Status:** IMPLEMENTATION_COMPLETE

#### Requirements
- Rectangle and other annotations are already canvas element in the pdf viewer component. Redaction annotation also rendered on the canvas element as like Rectangle annotation. 
- [ ] Update annotation processing in `updateCommentsCollection` to handle All the annotations. Should handle the redaction annotation also.
- [ ] Implement proper styling using markerFillColor, markerBorderColor, and markerOutlineOpacity
- [ ] Render the redaction annotation on the PDF Viewer as shown in the reference images:
  - [ ] Match the visual appearance demonstrated in `spec-util\redaction\images\Task-002-1.png` which shows how redaction and rectangle annotations are rendered in the Adobe PDF viewer
  - [ ] Implement similar rendering in our PDF viewer as shown in `spec-util\redaction\images\Task-002-2.png`
These screenshots serve as the exact visual specification for how the redaction annotations should appear when correctly implemented.

#### Acceptance Criteria:
1. Redaction annotations render correctly on PDF pages
2. **Visual appearance precisely matches the reference screenshots in `spec-util\redaction\images\`, including color, opacity, and border styling**
3. Redaction annotations apply the specific styling properties correctly
4. Implementation follows the same pattern as rectangle annotations
5. Annotations display properly at all zoom levels and page positions


## Task ID: TASK-001
### Summary: Implement Redaction Annotation Class
**Status:** IMPLEMENTATION_COMPLETE
#### Context
I need to add the New annotation called Redaction Annotation. Redaction Annotation similar to Rectangle annotation.
#### Requirements
- [ ] Implement IRedactionAnnotation interface with below properties
    shapeAnnotationType: string;
    author: string;
    modifiedDate: string;
    subject: string;
    note: string;
    bounds: any;
    rotateAngle: string;
    isLocked: boolean;
    annotName: string;
    pageNumber: number;
    annotationSelectorSettings: AnnotationSelectorSettingsModel;
    customData: object;
    overlayText: string;
    fontColor: string;
    fontSize: number;
    fontFamily: string;
    textAlign: string;
    markerFillColor: string;
    markerBorderColor: string;
    markerOutlineOpacity: number;
    annotationSettings: any;
    allowedInteractions: any;
    isPrint: boolean;
- [ ] Create class RedactionAnnotation to perform redaction Annotation code changes.
- [ ] Add the redaction-specific properties to the PdfAnnotationBaseModel interface in the Interfaces/base-annotation-interfaces.ts file
- [ ] Create property for redactionAnnotationModule in Annotation class
- [ ] Please add a new item for redaction annotation in the PdfAnnotationType enum.
#### Acceptance Criteria:
1. IRedactionAnnotation interface should contain all specified properties with proper TypeScript typing
2. RedactionAnnotation class should extend functionality similar to RectangleAnnotation but with redaction-specific features
3. PdfAnnotationBaseModel interface should include all redaction-specific properties
4. Annotation class should have a property for redactionAnnotationModule
5. Redaction annotations should work similar to Rectangle annotations but with additional redaction capabilities
#### Implementation Details:
1. Create the RedactionAnnotation class in sfpdfviewer/pdfviewer/annotation/redaction-annotation.ts
2. Update PdfAnnotationBaseModel interface in sfpdfviewer/Interfaces/base-annotation-interfaces.ts
3. Add redactionAnnotationModule property to the Annotation class and initialize it
4. Ensure proper rendering and handling of redaction annotations
