# PDF.js Minified Distribution With Viewer

### Annotation fix:
* this release includes a fix to gracefully handle exceptions during annotation creation caused by invalid URIs on  Edge.
* Without this fix, a single malformed annotation (e.g. http:myurl.9a) leads to an error on Microsoft Edge, resulting in the document not being rendered at all.
* This fix skips simply skips annotations that threw an error during annotation creation. Therefore, all URIs that could be properly parsed will appear in the PDF without causing the overall PDF creation to fail.


```
get annotations() {
    var annotations = [];
    var annotationRefs = this.getInheritedPageProp('Annots') || [];
    for (var i = 0, n = annotationRefs.length; i < n; ++i) {
      var annotationRef = annotationRefs[i];
      
      try {
        var annotation = AnnotationFactory.create(this.xref, annotationRef,
                                    this.pdfManager,
                                    this.idFactory);
        if (annotation) {
        annotations.push(annotation);
        }
      } catch (ex) {
        console.error('PDFDocument: cannot create annotation', ex);
      }
    }
    return shadow(this, 'annotations', annotations);
    },
};
```


PDF.js is a Portable Document Format (PDF) library that is built with HTML5.
Its goal is to create a general-purpose, web standards-based platform for
parsing and rendering PDFs.

This is a pre-built version of the PDF.js source code.  I was unable to find
a distribution of PDF.js that was both minified and included the viewer.html
component.  This is essentially just the files produced by running:
`gulp minified` against the upstream repo.

See https://github.com/mozilla/pdf.js for learning and contributing.
