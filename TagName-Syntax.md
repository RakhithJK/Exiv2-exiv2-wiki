Tags in Exiv2 are identified by a 3 part tuple in the syntax Family.Group.TagName.  For Exif.Image.Make.

The Exif tags are in a hierarchy.  At the top are the ones defined by Adobe and are labelled.  Exif.Image.Tag.  The Exif committee a defined a new tag in the Adobe section called ExifTag (0x8769).  This introduces an IFD in the group Exif.Photo.Tag.  The Exif people defined a tag called MakerNote (0x927c) which introduces the maker's IFD.  Exif.Canon.Tag is in the Canon MakerNote, and it goes deeper.  Exif.CanonCS.Tag is in the "Camera Settings" stored in the Canon MakerNote.  

![ExifTagNames](https://user-images.githubusercontent.com/529982/131121319-5d4dfcb6-2cd2-49ce-8216-73a311cb61e1.png)


IPTC is simpler.  It's a two level structure.  There are about 9 "sections" such as Envelope and Application2 for which the spec defines the tags.

XMP is also heirarchical because it's XML. The syntax of XML tags is handled by (and documented by) Adobe.

