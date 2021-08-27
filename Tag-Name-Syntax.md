Tags in Exiv2 are identified by a 3 part tuple in the syntax _**Family.Group.TagName**_.  For Example _**Exif.Image.Make**_.

The **Exif** tags are in a hierarchy.  The _**.Group.**_ indicated the IFD in which the tag was found.  At the top are tags defined by Adobe in the Tiff_EP specification. The are of the style: Exif.Image.Tag.

Exif defines a tag for the Image section called ExifTag (0x8769).  This introduces an IFD in the group Exif.Photo.Tag.

Exif defines a tag called MakerNote (0x927c) which introduces the maker's IFD.  Exif.Nikon.TagName are tags in the Nikon MakerNote.  There are binary tags in the maker note.  For example: Exif.NikonPc.TagName are in the _**PictureControl**_" record stored in the Nikon MakerNote.  

![ExifTagNames](https://user-images.githubusercontent.com/529982/131121319-5d4dfcb6-2cd2-49ce-8216-73a311cb61e1.png)

Exif Metadata is stored in a Tiff formatted sub-file of the image.  The arrangements for the storage of this sub-file are defined by the file format.  The Exif metadata is independent of the file format.  The following drawing illustrates the design of a Tiff file.

![Tiff File Layout](https://user-images.githubusercontent.com/529982/131124453-afc30d02-c845-42a3-9a84-299b569b712c.png)

The **IPTC** tags are simpler.  IPTC a two level structure.  There are 9 "sections" such as Envelope and Application2 for which the spec defines the tags.

XMP is a heirarchical tree because it's XML. The syntax of XML tags is handled by (and documented by) the Adobe XMP SDK.
