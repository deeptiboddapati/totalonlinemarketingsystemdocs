---
sidebar_label: 'How to pick block attributes'
sidebar_position: 2
---

Type of Content
Images:
    prefixes:
        imageUrl,
        imgAlt,
        imgID,
    Modifier:
        Hero Image: _featured
        Icon: _icon
Text:
    Page Headline: headline
    Text content of the block: content
Buttons:
    Primary:
        urlPrimary
        ctaPrimary
    Secondary:
        urlSecondary
        ctaSeconday

WordPress Docs: https://developer.wordpress.org/block-editor/reference-guides/block-api/block-metadata/

Docs for attributes: https://developer.wordpress.org/block-editor/reference-guides/block-api/block-attributes/

1. Set the source to HTML for Rich Text blocks that allow formatting.

Workarounds:
1. If you add attributes to a block after it's already been added to the editor, it won't recognize it as the same block so it'll hide it. Comment the block registration PHP code out, refresh the editor, then uncomment and refresh the editor again and the block should work. 

When naming block attributes follow this structure:

