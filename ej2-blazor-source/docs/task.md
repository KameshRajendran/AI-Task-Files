# Tasks


## Task ID: TASK-009: Implement Redaction Page Mark Selection and Save Support
**Status:** NEEDS_PLAN

---

### Summary
Implement a feature to select specific pages for redaction through a Redaction Page Mark panel. The panel should support Current Page, Odd Pages, Even Pages, and Specific Pages selection, and send the selected pages for redaction on save. The functionality must work on both desktop and mobile layouts.

---

### Requirements
1. When clicking a button with the class `e-pv-redact-pages-container`, make the `RedactionPageMark` visible.
2. Display 4 radio buttons in the `RedactionPageMark` panel:
   - **Current Page**: Use the current page number from `Toolbar.razor`.
   - **Odd Pages**: Use the total page count from `Toolbar.razor`, and select all odd page numbers.
   - **Even Pages**: Use the total page count from `Toolbar.razor`, and select all even page numbers.
   - **Specific Pages**: Parse the input from `PageRangeValue`, which may be a range like `1-5,7,10-12`, and generate a list of valid page numbers. Invalid formats should be ignored.
3. On clicking the Save button:
   - Prepare a `List<int> pages` based on the selected radio option.
   - Call `await Parent.RedactPagesAsync(pages);` to send the list.
4. Ensure the same functionality is available and works correctly on mobile devices as well.

---

### Implementation Steps
1. Attach a click event listener to the button with class `e-pv-redaction-panel-container` to toggle `RedactionPageMark` visibility.
2. Create four radio button options within the `RedactionPageMark` UI.
3. On radio button selection:
   - **Current Page**: Fetch and store the current page number.
   - **Odd Pages**: Generate a list of odd pages up to the total pages.
   - **Even Pages**: Generate a list of even pages up to the total pages.
   - **Specific Pages**: Parse the `PageRangeValue` string into a list of integers, validating proper formats.
4. Handle Save button click:
   - Based on the selected radio option, build the `pages` list.
   - Call `await Parent.RedactPagesAsync(pages);`.
5. Ensure responsive behavior:
   - Adjust the `RedactionPageMark` layout for mobile screens.
   - Test the behavior across both desktop and mobile views.
6. Dont implement any JS code. We can handle JS code later.
---

### Acceptance Criteria
- Clicking the button with `e-pv-redaction-panel-container` shows the `RedactionPageMark`.
- 4 radio options are visible: Current Page, Odd Pages, Even Pages, Specific Pages.
- Selecting each option correctly prepares the corresponding list of page numbers.
- Clicking the Save button successfully sends the selected page numbers to `Parent.RedactPagesAsync`.
- On invalid Specific Pages input, invalid entries are ignored without breaking functionality.
- Feature works seamlessly on both desktop and mobile devices.

 

## Task ID: TASK-008: Implement RedactionPageMark.razor Component  
**Status:** NEEDS_PLAN  

### Summary  
Implement a new `RedactionPageMark.razor` component that provides a user interface for selecting pages to apply redaction marks in the PDF Viewer. The component should follow the same design and localization pattern used in the existing `RedactionPropertiesDialog`.

### Requirements  
- Use a dialog box layout similar to `RedactionPropertiesDialog`.
- Support both **desktop** and **mobile** responsive views.
- Provide four page selection options using **SfRadioButton**:
  1. Current Page
  2. Odd Pages Only
  3. Even Pages Only
  4. Specific Pages
    - When selected, a row containing the label **Page range** and a **SfTextBox** input should appear.
    - The label should be aligned to the left and the textbox on the right.
    - When unselected, the label and textbox should be hidden.
- All text labels and placeholders must support **localization** using `PdfViewerLocaleKeys`.

### Implementation Steps  
1. Create `RedactionPageMark.razor` component.
2. Add a dialog container similar to `RedactionPropertiesDialog`.
3. Add four radio buttons using `SfRadioButton`, grouped under the same name.
4. Implement `Change` event logic to ensure only one radio is selected at a time.
5. When the **Specific Pages** option is selected:
   - Render a horizontal layout with a localized label (`PageRange`) and a textbox (`SfTextBox`) with placeholder (`EnterPageRange`).
   - Ensure this section is hidden when another radio option is selected.
6. Make the layout responsive for mobile and desktop using appropriate CSS classes Please refer the RedactionPropertiesDialog.razor component.
7. Localize all user-visible strings using `PdfViewerLocaleKeys`.

### Acceptance Criteria  
- [ ] Dialog UI layout matches the style of `RedactionPropertiesDialog`.
- [ ] All four radio options are present and mutually exclusive.
- [ ] Selecting “Specific Pages” reveals a text input with proper alignment and hides it otherwise.
- [ ] Component works and looks good on both desktop and mobile screens.
- [ ] All labels, radio button text, and placeholders are localized via `PdfViewerLocaleKeys`.
- [ ] Code is peer reviewed and passes existing Blazor UI validation.
 


## Task ID: TASK-007: Implement Save Button Functionality in `RedactionPropertiesDialog`  
**Status:** NEEDS_PLAN

---

### Summary  
Implement the `Save` button support in `RedactionPropertiesDialog` to persist the redaction settings. This involves checking whether a specific redaction annotation is selected, and accordingly updating the redaction annotation via JS or assigning default redaction settings globally in the viewer.

---

### Requirements  
1. Add a **boolean property** to confirm whether the redaction dialog is opened to modify an existing redaction annotation or to set general settings.
2. In the `OpenRedactionPropertiesDialog` method, use the below JS interop line to check if a redaction annotation is selected:

   ```csharp
   object returnValue = await this.Parent.InvokeMethod<object>(
       "sfBlazor.SfPdfViewer.invokeMethod", 
       true, 
       new object[] { this.Parent.dataId, "getRedactionSettings", "annotation", null });
   ```

   - If `returnValue` is not `null`, a redaction annotation is selected.
   - If `returnValue` is `null`, then it is a general settings update.

3. In the `Save` button click handler:
   - **Case A**: Annotation is selected
     - Send the updated `RedactionSettingsInfo` to JavaScript using interop (e.g., `updateRedactionSettings`) to update the selected redaction annotation on the client side.
     - Example:

       ```csharp
       await this.Parent.InvokeMethod<object>(
           "sfBlazor.SfPdfViewer.invokeMethod", 
           true, 
           new object[] { this.Parent.dataId, "updateRedactionSettings", "annotation", RedactionSettingsInfo });
       ```

   - **Case B**: No annotation selected
     - Assign each property in `RedactionSettingsInfo` to `PdfViewerRedactionSettings`.
     - This ensures new redaction annotations use the updated settings globally.

4. After saving:
   - Close the `RedactionPropertiesDialog` by updating `IsVisible` (for desktop) or `DisplayStyle` (for mobile).

---

### Implementation Steps  
1. Add a new **bool field** (e.g., `_isAnnotationSelected`) in the component or state management area.
2. Update `OpenRedactionPropertiesDialog()`:
   - Call JS interop method to get selected annotation settings.
   - If response is not null, set `_isAnnotationSelected = true`.
3. In the Save method:
   - If `_isAnnotationSelected` is `true`:
     - Send `RedactionSettingsInfo` to JS to update the selected redaction annotation.
   - Else:
     - Assign each field in `RedactionSettingsInfo` to `PdfViewerRedactionSettings`.
4. Close the dialog.
5. Ensure all changed values persist visually on the client.

---

### Acceptance Criteria  
- [ ] `Save` button updates the selected annotation if one is selected.
- [ ] If no annotation is selected, `Save` updates global settings in `PdfViewerRedactionSettings`.
- [ ] `RedactionPropertiesDialog` closes after saving.
- [ ] No JS or deserialization exceptions occur.
- [ ] Redaction setting changes reflect correctly on the UI.

 

## Task ID: TASK-006: Implement `_redactionPanel` Click Logic in Desktop and Mobile Redaction Toolbar  
**Status:** NEEDS_PLAN
---
### Summary  
Implement the logic for `_redactionPanel` click in both Desktop and Mobile redaction toolbars to open the `RedactionPropertiesDialog`. This includes invoking a JS method to retrieve redaction settings, deserializing the returned object, updating the `RedactionSettingsInfo`, and displaying the dialog with proper bindings for each redaction setting component.

---

### Requirements  
1. `RedactionPropertiesDialog` must be in a **closed state** by default.  
2. On `_redactionPanel` button click:
   - Call the existing `OpenDialog` method.
   - Display the `RedactionPropertiesDialog` using its `IsVisible` (Desktop) or `DisplayStyle` (Mobile) property.
3. Before opening the dialog:
   - Invoke a **JavaScript method** (e.g., `getredacionSettings`) via interop.
   - The JS method should return the following properties:
     - `FillColor`
     - `OutlineColor`
     - `FillOpacity`
     - `UseOverlayText`
     - `FontFamily`
     - `FontSize`
     - `FontColor`
     - `TextAlign`
     - `RepeatText`
     - `OverlayText`
4. Deserialize the JS return value into a `RedactionSettingsInfo` object.
5. Assign the deserialized properties to `RedactionSettingsInfo`.
6. If the return value is `null`, assign properties from `PdfViewerRedactionSettings`.
7. After assignment, display the dialog using `IsVisible` or `DisplayStyle`.
8. Ensure the following UI components are **bound** with `RedactionSettingsInfo`:
   - `FontFamilyDropDown`
   - `FontSizeDropDown`
   - `FillColorPicker`
   - `FontColorPicker`
   - `FillColorButton`
   - `FontColorButton`
   - `FontFamilyTooltip`
   - `OutlineColorPicker`
   - `FillOpacitySlider`
9. If any of the listed redaction properties are missing in `PdfViewerRedactionSettings`, implement and integrate them appropriately.

---

### Implementation Steps  
1. **Update Click Handler** for `_redactionPanel` in both Desktop and Mobile toolbars.  
2. Inside the click method:
   - Call JS interop method like:

     ```csharp
     object redactsettingInfo = await Parent.InvokeMethod<object>("getredacionSettings", true, "annotationModule, getredacionSettings ", null);
     ```

3. **Deserialize the return value**:

   ```csharp
   if (redactsettingInfo != null)
   {
       RedactionSettingsInfo settings = JsonSerializer.Deserialize<RedactionSettingsInfo>(redactsettingInfo.ToString());
       if (settings != null)
       {
           RedactionSettingsInfo = settings;
       }
   }
   else
   {
       RedactionSettingsInfo = PdfViewerBase.RedactionSettings;
   }
   ```

4. Ensure each property (`FontFamily`, `FillColor`, etc.) is **properly assigned** to `RedactionSettingsInfo`.  
5. Use `IsVisible` or `DisplayStyle` to **open the dialog**.  
6. **Bind each component** to the relevant property in `RedactionSettingsInfo`.  
7. **Extend `PdfViewerRedactionSettings`** to include any missing properties and update its usage throughout the viewer (initialization, saving/loading settings, etc.).

---

### Acceptance Criteria  
- [ ] Clicking `_redactionPanel` shows `RedactionPropertiesDialog` with all values preloaded.  
- [ ] Redaction settings are fetched from JS and correctly reflected in the dialog.  
- [ ] When JS returns `null`, default values from `PdfViewerRedactionSettings` are used.  
- [ ] All UI controls are bound with `RedactionSettingsInfo` values.  
- [ ] Implementation is functional across both **desktop** and **mobile** layouts.  
- [ ] Missing redaction properties (if any) are added to `PdfViewerRedactionSettings` and used correctly.

 


## Task ID: TASK-005: Add Appearance Tab to Redaction Property Dialog  
**Context:**  
The **General** tab for redaction settings is now complete for both **desktop** and **mobile**.  
Next, we will implement the **Appearance** tab as the second tab in the same dialog.

---

### Target Tabs in SfTab  
1. **General** – Completed  
2. **Appearance** – This implementation

---

### Appearance Tab – Requirements

| Field              | Component         | Binding Property                     | Notes                                                |
|-------------------|-------------------|--------------------------------------|------------------------------------------------------|
| Outline Color      | `SfColorPicker`    | `SelectedRedaction.OutlineColor`     | Bind to redaction settings object                   |
| Fill Color         | `SfColorPicker`    | `SelectedRedaction.FillColor`        | Already in use for General tab, reused here         |
| Fill Opacity       | `SfSlider`         | `SelectedRedaction.FillOpacity`      | Range: 0 to 100 or 0.0 to 1.0, bind accordingly      |

---

### Integration Requirements

- **Localization**  
  - All labels, placeholders, and tooltip text must use `@L["KeyName"]`
  - Add new keys in `PdfViewerLocaleKeys` (e.g., `OutlineColor`, `FillOpacity`)

- **Bindings**  
  - All properties are part of `SelectedRedaction`, same as General tab
  - Ensure two-way binding (`@bind-Value` or equivalent)

- **Component Layout**  
  - Use same layout classes (`.e-pv-property-row`, `.e-pv-label`) as in General tab
  - Consistency with `FormDesignerPropertiesDialog.razor` is essential

- **Responsiveness**  
  - Ensure mobile and desktop view adapt styles via media queries or CSS classes
  - Test mobile experience using simulated viewport and actual devices

---

### Example Markup Snippet (Appearance Tab)

```razor
<TabItem Header="@L[PdfViewerLocaleKeys.Appearance]">
    <ChildContent>
        <div class="e-pv-properties-section">
            <!-- Outline Color -->
            <div class="e-pv-property-row">
                <label>@L[PdfViewerLocaleKeys.OutlineColor]</label>
                <SfColorPicker @bind-Value="SelectedRedaction.OutlineColor" CssClass="e-pv-color-picker" />
            </div>

            <!-- Fill Color -->
            <div class="e-pv-property-row">
                <label>@L[PdfViewerLocaleKeys.FillColor]</label>
                <SfColorPicker @bind-Value="SelectedRedaction.FillColor" CssClass="e-pv-color-picker" />
            </div>

            <!-- Fill Opacity -->
            <div class="e-pv-property-row">
                <label>@L[PdfViewerLocaleKeys.FillOpacity]</label>
                <SfSlider @bind-Value="SelectedRedaction.FillOpacity" Min="0" Max="100" ValueType="ValueType.Percentage" />
            </div>
        </div>
    </ChildContent>
</TabItem>
```

---

### ✅ Final Checklist

- [ ] Components are rendered correctly in both desktop and mobile layout
- [ ] All labels are localized using `PdfViewerLocaleKeys`
- [ ] All values are two-way bound to `SelectedRedaction` model
- [ ] CSS icons and spacing are consistent with Form Designer properties panel
- [ ] Slider is responsive and styled correctly in mobile view

 



## Task ID: TASK-004 - Design Redaction Properties Panel for Desktop  
**Status:** NEEDS_PLAN

### Summary  
Design and implement a **Redaction Properties Panel** for editing redaction settings in the PDF Viewer desktop UI. The panel should use a dialog with two tabs (starting with General tab). The layout, style, and binding logic should mirror the existing `FormDesignerPropertiesDialog.razor` implementation.

### Requirements  
- Implement a `SfDialog` with `SfTab` inside.
- Focus on **desktop** support only in this task.
- Tab 1: **General** tab with the following components (as per redaction features).
- Tab 2: Placeholder for future use.
- All fields should bind to a `SelectedRedactionSettings` object of type `RedactionSettingsInfo` or equivalent.
- All text and labels must support **localization** (`PdfViewerLocaleKeys`).
- **Use proper CSS icons and values**, just like in `FormDesignerPropertiesDialog`.
- Expected redaction property diagram like below (SfPdfViewer\docs\images\Redaction Properties_Desktop.png)
- Use the RenderFragment approach as like FormDesignerPropertiesDialog.razor to utilize the same component for both Desktop and Mobile

#### General Tab Components  
1. **Redacted Area FillColor**  
   - `SfColorPicker` → `FillColor`

2. **Use Overlay Text**  
   - `SfCheckBox` → `UseOverlayText`

3. **Font Family**  
   - `SfDropDownList` → `FontFamily`  
   - Options: Helvetica, Courier, Symbol, Times New Roman

4. **Font Size**  
   - `SfComboBox<int>` → `FontSize`

5. **Font Color**  
   - `SfColorPicker` → `FontColor`

6. **Text Alignments**  
   - Follow `FormDesignerPropertiesDialog.razor` pattern  
   - `TextAlign` → Left, Center, Right (icon-based)

7. **Repeat Overlay Text**  
   - `SfCheckBox` → `RepeatText`

8. **Custom Text (Overlay Text)**  
   - `SfTextBox` → `OverlayText`

9. **Footer Buttons**  
   - `SfButton` – **Cancel** and **Save**  
   - `Cancel`: close dialog  
   - `Save`: commit changes to annotation settings

10. **Icon Usage in CSS**  
   - Use proper icon classes and sprite references  
   - Ensure all icons for checkboxes, dropdowns, buttons match existing Form Designer styles  
   - Reference `.e-pv-` prefixed classes for consistency

### Implementation Steps  
1. **Component Creation**  
   - Create `RedactionPropertiesDialog.razor` file  
   - Create `RedactionSettingsInfo.cs` model if not already present

2. **Tab Setup**  
   - Add `SfTab` inside `SfDialog`  
   - Tab 1: Implement General tab UI  
   - Tab 2: Leave placeholder

3. **Bindings and Model**  
   - Create and bind `SelectedRedactionSettings` property  
   - Wire up all components with two-way binding

4. **Localization**  
   - Add text keys to `PdfViewerLocaleKeys`  
   - Use `@L("KeyName")` for all static text (tooltip, label, placeholder)

5. **Styling & Icon Integration**  
   - Match UI layout and paddings used in `FormDesignerPropertiesDialog`  
   - Reuse CSS classes like `.e-pv-color-picker`, `.e-pv-checkbox-label`, `.e-pv-font-icon`, etc.  
   - Icons must be integrated via sprite or inline classes, consistent with existing design

6. **Functionality**  
   - Implement `Save()` and `Cancel()` handlers  
   - Save: update PDF viewer redaction settings  
   - Cancel: discard and close

### Acceptance Criteria  
- General tab renders with all required components and is functionally interactive.
- Buttons perform expected actions (`Cancel` and `Save`).
- All components correctly update `SelectedRedactionSettings`.
- Icons and layout match PDF Viewer styling conventions.
- Localization works across all text labels/tooltips.
- Tab 2 is empty but present for future expansion.
 


## Task ID: TASK-003 - Implement Redaction Toolbar for Mobile Devices  
**Status:** NEEDS_PLAN

### Summary  
Develop the **Mobile Redaction Toolbar** for PDF Viewer, following the same design and structure as the existing **Form Designer Mobile Toolbar**. This toolbar will be displayed when the redaction icon is tapped from the mobile toolbar, showing specific redaction actions aligned to the left.

### Requirements  
1. **Add Mobile Redaction Toolbar Icon**  
   - Add a **redaction icon** to the existing mobile toolbar component.
   - The icon should match the one used in the desktop version (`.e-pv-redaction-icon`).
   - Tooltip: `"Redaction"`
   - Utize the all the icons used in the Desktop Redaction tool.

2. **Create Mobile Redaction Toolbar Component**  
   - Develop a dedicated component to represent the redaction toolbar for mobile devices.
   - Structure and styling should follow the mobile **Form Designer toolbar** implementation.
   - Mobile redaction toolbar should be positioned on the botom of the PDF Viewer component. 
   - Reference image: `SfPdfViewer\docs\images\FormDesignerToolbarUI_Mobile.png`

3. **Display Logic**  
   - On tapping the redaction icon:
     - Show the redaction toolbar.
   - On second tap or close:
     - Hide the toolbar.
   - Ensure this toggling logic matches behavior of other mobile toolbars like Annotation and Form Designer.

4. **Toolbar Items (Aligned Left)**  
   Add the following items to the mobile redaction toolbar:
   - **Mark for redaction**  
     - Tooltip: `"Mark for redaction"`  
     - Icon: `.e-pv-mark-for-redaction-icon`
   - **Redact Pages**  
     - Tooltip: `"Redact Pages"`  
     - Icon: `.e-pv-redact-pages-icon`
   - **Redaction Panel**  
     - Tooltip: `"Redaction Panel"`  
     - Icon: `.e-pv-redaction-panel-icon`
   - **Redact**  
     - Tooltip: `"Redact"`  
     - Icon: `.e-pv-redaction-icon`
   - **Delete**  
     - Tooltip: `"Delete"`  
     - Icon: `.e-pv-delete-icon` (if already defined in Form Designer, reuse)

5. **Icon and Locale Reuse**  
   - Reuse existing icons and locale keys defined for desktop toolbar.
   - Ensure localization works in mobile toolbar.

6. **Mobile-Optimized Layout**  
   - Stack items vertically or align them to the left side.
   - Ensure the layout is responsive and supports small-screen touch interactions.

7. **Component Lifecycle Handling**  
   - Properly dispose of redaction toolbar instance when:
     - Navigating away from redaction mode.
     - Destroying or reinitializing viewer instance.
   - Ensure internal items are disposed to prevent memory leaks.

8. **Reference Existing Form Designer Toolbar Implementation**  
   - Follow same lifecycle, rendering, and structure patterns used in:
     - `FormDesigner Mobile Toolbar`
     - File location: `SfPdfViewer\docs\images\FormDesignerToolbarUI_Mobile.png`
- Expected redaction property diagram like below (SfPdfViewer\docs\images\Redaction Properties_Desktop.png)


### Implementation Steps  
1. Add redaction icon entry in mobile toolbar component.
2. Create `MobileRedactionToolbar` component.
3. Copy structural pattern from `MobileFormDesignerToolbar`.
4. Add toolbar items and bind toggle events.
5. Add appropriate tooltips and icon classes.
6. Implement `dispose` method for both toolbar and items.
7. Validate localization strings from `PdfViewerLocaleKeys`.
8. Test UI on various screen sizes and mobile devices.

### Acceptance Criteria  
- Mobile redaction icon appears in toolbar.
- Redaction toolbar toggles correctly on icon click.
- All 5 tools appear aligned left with correct icons and tooltips.
- Toolbar and its items are disposed properly.
- UI behaves responsively and mimics Form Designer mobile layout.
- No memory leaks or broken UI interactions on mobile.

Let me know if you also need a visual spec or split of sub-tasks for better tracking.



## Task ID: TASK-002 - Add and Integrate Redaction Toolbar in PDF Viewer  
**Status:** NEEDS_PLAN
### Summary  
Create and integrate a new Redaction Toolbar in the Toolbar component of the PDF Viewer. The toolbar will have four redaction-related options and a close button, and it will only be enabled in non-device mode (`!isDeviceMode`). Icon keys should also be added to the `PdfViewerLocaleKeys` for localization support.
### Requirements  
- Add a new redaction toolbar icon to the main toolbar.
- Create the `RedactionToolbar` component with four left-aligned redaction tools:
  - **Mark for redaction** (`.e-pv-mark-for-redaction-icon`)
  - **Redact Pages** (`.e-pv-redact-pages-icon`)
  - **Redaction Panel** (`.e-pv-redaction-panel-icon`)
  - **Redact** (`.e-pv-redaction-icon`)
- Include a left-aligned **Close** button (reused from Annotation/Forms).
- Show/Hide the toolbar when the redaction icon is toggled (similar to `AnnotationToolbar` and `FormDesignerToolbar`).
- Only implement the toolbar for `!isDeviceMode`.
- Add localized keys for new redaction icons in `PdfViewerLocaleKeys`.
- Please refer the images of existing FormDesignerToolbar( SfPdfViewer\docs\images\FormDesignerToolbarUI.png)
- Expected redaction toolbar (SfPdfViewer\docs\images\ExpectedRedactionToolbar.png)
### Implementation Steps  
1. **Add Redaction Toolbar Icon to Main Toolbar**  
   - Update the main toolbar config to include the redaction icon with tooltip `Redaction`.
2. **Create `RedactionToolbar` Component**  
   - Structure similar to existing `AnnotationToolbar` or `FormDesignerToolbar`.
   - Add logic to render only when redaction toolbar toggle is active and `!isDeviceMode`.
3. **Add Toolbar Items (Aligned Left)**  
   - **Mark for redaction**: Use `.e-pv-mark-for-redaction-icon`
   - **Redact Pages**: Use `.e-pv-redact-pages-icon`
   - **Redaction Panel**: Use `.e-pv-redaction-panel-icon`
   - **Redact**: Use `.e-pv-redaction-icon`
   - Add **Close** icon (already handled in Annotation).

4. **Toolbar Visibility Handling**  
   - Reuse the toggle mechanism logic from `AnnotationToolbar` and `FormDesignerToolbar`.

5. **Add Locale Keys**  
   - Add keys in `PdfViewerLocaleKeys` for:
     - `Redaction`
     - `MarkForRedaction`
     - `RedactPages`
     - `RedactionPanel`
     - `Redact`
     - `Close`

6. **Test on Non-Device Mode Only (`!isDeviceMode`)**  
   - Ensure toolbar renders and functions only when the device mode flag is false.

### Acceptance Criteria  
- Redaction toolbar icon appears in the main toolbar.
- Clicking the redaction icon toggles the visibility of the `RedactionToolbar`.
- Toolbar has four redaction items and a close button, all aligned left.
- Toolbar only renders in `!isDeviceMode`.
- Locale keys are correctly registered and applied.
- Functionality is consistent with existing Annotation and FormDesigner toolbars.


## Task ID: TASK-001 - Add redaction annotation properties to PdfAnnotation Class
### Summary
The task involves implementing redaction annotation by adding redaction-specific properties to the existing PdfAnnotation class.

**Status:** IMPLEMENTATION_COMPLETE
#### Context
The system already supports rendering and creating annotations through the PdfAnnotation class. This task requires extending the class with additional properties specifically for redaction functionality, including:
- OverlayText
- RepeatText
- MarkerOutlineColor
- MarkerFillColor
- MarkerOpacity
-Requirements

#### Requirements
- Add below properties to PdfAnnotation Class
    /// <summary>
    /// Gets or sets the overlay text displayed on the annotation.
    /// </summary>
    public string OverlayText { get; set; }

    /// <summary>
    /// Gets or sets a value indicating whether the overlay text should be repeated.
    /// </summary>
    public bool RepeatText { get; set; }
  
    /// <summary>
    /// Gets or sets the outline color of the marker annotation.
    /// </summary>
    public string MarkerOutlineColor { get; set; }

    /// <summary>
    /// Gets or sets the fill color of the marker annotation.
    /// </summary>
    public string MarkerFillColor { get; set; }

    /// <summary>
    /// Gets or sets the opacity of the marker annotation.
    /// </summary>
    public string MarkerOpacity { get; set; }


 #### Acceptance Criteria:
 