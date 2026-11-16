# Scribus Brings Publication Layout Under Your Control

A scribus pdf workflow connects editable page design with a deliverable that readers and printers can review.
Use scribus software to arrange text, photographs, and graphics in brochures, magazines, books, and posters.

[![Design with Scribus](https://img.shields.io/badge/Design%20with-Scribus-E87524?style=plastic&labelColor=383838)](https://dtp-scribus.github.io/Scribus-Software/)

## Define the handoff before arranging the page

A publication has two audiences: the person reading it and the person producing it.
Agree on page dimensions, binding, bleed, and the intended PDF specification before detailed layout work.

- Ask the printer which color profile and PDF standard the job requires.
- Decide whether the reader needs a compact screen copy, a print copy, or both.
- Keep the editable document, source images, and font information together.
- Reserve time to inspect an exported PDF after the layout passes its own checks.

Treat an approved proof as a specific revision with a date and a clear filename.
A later text correction deserves another export and review.

## Build the reading order with frames

In scribus desktop publishing, a page is assembled from objects with deliberate positions and dimensions.
Text frames establish the reading path, while image frames provide space for illustrations and photographs.

### Separate recurring design from changing content

Use master pages for recurring furniture such as folios, headers, and background elements.
Keep issue-specific stories and photographs on the document pages.
Paragraph and character styles make recurring text roles easier to revise consistently.

Link text frames when a story continues across columns or pages.
After editing the copy, check the entire chain for overflow and awkward endings.
A shorter headline can improve a spread more effectively than shrinking every element.

![A newsletter spread with linked text columns and an aligned photograph](dtd/foto1.jpg)

### Add a heading through the Scripter

Run this Python example inside Scribus with an open document and a page large enough for the frame.
It changes the document units to millimeters and adds a heading on the current page.
The [text-frame API](https://github.com/scribusproject/scribus/blob/master/doc/en/scripterapi-textframes.html) documents the text operations.

```python
import scribus
if not scribus.haveDoc():
    raise RuntimeError("Open a document first.")
scribus.setUnit(scribus.UNIT_MILLIMETERS)
frame = scribus.createText(20, 20, 170, 25)
scribus.insertText("Community Bulletin", 0, frame)
scribus.setFontSize(22, frame)
```

## Preserve the editable publication

Keep the native SLA document as the working source for later corrections.
A PDF is the distribution artifact; retain the layout and its referenced assets separately.

The supplied source snapshot also contains historical format material:

- [scribus.dtd](dtd/scribus.dtd) describes an older document structure.
- [scribus13.xml](dtd/scribus13.xml) is a historical format test document.
- [style.ent](dtd/style.ent) defines style-related entities for that material.
- [namespaces.ent](dtd/namespaces.ent) declares namespace entities.
- [notations.ent](dtd/notations.ent) declares external media notations.
- [background.sla](OSX-package/inputs/background.sla) is legacy packaging artwork saved as a Scribus document.

These files help explain format history; they are not a validation recipe for current SLA documents.
For a dtp scribus project, preserve a working copy before opening older material for revision.

## Inspect typography and placed images together

Read the page at its intended size as well as at a zoom level suited to alignment checks.
A spread can look orderly while still containing small captions or an interrupted reading path.

- Check spelling, punctuation, and the last line of every linked story.
- Confirm that image cropping leaves the subject and caption in agreement.
- Review image resolution at the size actually used in the layout.
- Verify that the intended fonts are available and permitted for the planned output.

The source build locates text-related libraries through [FindHYPHEN.cmake](cmake/modules/FindHYPHEN.cmake) and [Findhunspell.cmake](cmake/modules/Findhunspell.cmake).
Those build modules provide dependency context rather than document-level proofreading settings.

![A close view of caption typography beside a cropped publication image](dtd/foto2.jpg)

## Match the PDF to its destination

Choose export settings for the recipient and inspect the resulting file independently.
Color management, font handling, image compression, and bleed all affect the handoff.

| Destination | Review priority | Decision to confirm |
| --- | --- | --- |
| Editorial proof | Complete text and correct page order | Which revision reviewers should annotate |
| Screen distribution | Readability and practical file size | Image compression and navigation needs |
| Commercial printing | Production specifications | Required PDF standard, profiles, bleed, and marks |

A PDF/X label alone does not establish that every detail matches a particular printer's requirements.
Use the preflight checks, resolve relevant findings, and obtain approval for the final production file.

### Export a review copy with Python

This example uses the active document's PDF settings and sets the output filename.
Run it through Scribus Scripter after reviewing those settings.
The [PDFfile reference](https://github.com/scribusproject/scribus/blob/master/doc/en/scripterapi-PDFfile.html) describes the export object and its options.

```python
import scribus
if not scribus.haveDoc():
    raise RuntimeError("Open the publication before exporting.")
review = scribus.PDFfile()
review.file = "bulletin-review.pdf"
review.save()
```

The example does not select a PDF/X standard or a printer profile.
An existing file with that output name may be replaced.

For source readers, [FindLCMS2.cmake](cmake/modules/FindLCMS2.cmake) locates the color-management library.
[Findpoppler.cmake](cmake/modules/Findpoppler.cmake) locates Poppler components used by the build for PDF import.

## Repeat a reviewed export script

For a Scribus build with Scripter support, the command line can select a Python script.
Assuming the preceding export example is saved as an existing local script, this command opens the publication for it:

```bash
scribus --python-script review_export.py -- publication.sla
```

The separator keeps the document filename out of the script's argument list, as shown in the [command-line parser](https://github.com/scribusproject/scribus/blob/master/scribus/scribusapp.cpp).
This invokes Scribus with its normal interface; it is not a promise of unattended headless conversion.
The script and document names above are examples, not files included in this snapshot.

## Read the source snapshot in context

The scribus open source project includes application code, build definitions, and platform packaging resources.
[AUTHORS](AUTHORS) records contributors, while [ChangeLog](ChangeLog) provides development history.
[BUILDING](BUILDING) contains build notes; [BUILDING_win32_cmake.txt](BUILDING_win32_cmake.txt) explicitly preserves outdated Windows guidance.
[CMakeLists.txt](CMakeLists.txt) defines the main CMake project, with dependency logic in [CMakeLists_Dependencies.cmake](CMakeLists_Dependencies.cmake).
Apple-specific build configuration appears in [CMakeLists_Apple.cmake](CMakeLists_Apple.cmake).
The AppImage resources include a launcher in [AppRun](AppImage-package/AppRun) and packaging steps in [bundle.sh](AppImage-package/bundle.sh).
The legacy macOS packaging material includes [Makefile](OSX-package/Makefile), [Info.plist](OSX-package/inputs/Info.plist), and [Scribus.pkg-Info.plist](OSX-package/inputs/Scribus.pkg-Info.plist).

The [GitHub mirror's status note](https://github.com/scribusproject/scribus/blob/master/README.md) explains that it is community maintained and unsupported by the Scribus team.

## Related Search Terms

scribus pdf, scribus software, scribus desktop publishing, dtp scribus, scribus open source, desktop-publishing, page-layout, typesetting, pdf, pdf-generation, typography, print-design, color-management, python, open-source
