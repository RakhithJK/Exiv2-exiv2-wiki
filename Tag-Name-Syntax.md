Tags in Exiv2 are identified by a 3 part tuple in the syntax _**Family.Group.TagName**_.  For Example _**Exif.Image.Make**_.  There are three families: Exif, Iptc and Xmp.

The **Exif** tags are in a hierarchy.  The _**Group**_ indicates the IFD in which the tag was found.  At the top are tags defined in the TIFF/EP specification and are named: Exif.Image.Tag.

Exif defines tags in the Image IFD called ExifTag (0x8769) and GPSTag (0x8825) which are both IFDs.  The tags in those IFDs are in Group _**Photo**_ and _**GPSInfo**_, so the tags are named: Exif.Photo.Tag and Exif.GPSInfo.Tag.

An IFD is an array of tags.  Multiple arrays can be chained.  The second array of tags in the Exif IFD normally contains thumbnail images.  The tags in this array are named: Exif.Thumbnail.Tag.

Exif defines a tag called MakerNote (0x927c) which introduces the camera manufacturer's data.  This is normally a short header followed by an IFD.  Exif.Nikon.Tag are tags in the Nikon MakerNote.  In addition to the Exif formatted tags, maker notes usually contain binary structures.  For example: Exif.NikonPc.TagName are in the _**PictureControl**_" record stored in the Nikon MakerNote.  

![ExifTagNames](https://user-images.githubusercontent.com/529982/131121319-5d4dfcb6-2cd2-49ce-8216-73a311cb61e1.png)

Exif Metadata is stored in a Tiff formatted sub-file of the image.  The arrangements for the storage of this sub-file are defined by the file format.  The Exif metadata is independent of the file format.  The following drawing illustrates the design of a Tiff file.

![Tiff File Layout](https://user-images.githubusercontent.com/529982/131124453-afc30d02-c845-42a3-9a84-299b569b712c.png)

**IPTC** tags are simpler.  IPTC is two level structure.  There are 9 "sections" such as Envelope and Application2 for which the spec defines the tags.  The Exiv2 Family.Group.Tag maps to Iptc.Section.Tag.  For example:  Iptc.Envelope.ModelVersion and Iptc.Application2.Caption.

XMP is represented in XMP and is therefore a heirarchical tree.
The Exiv2 Family.Group.Tag maps to Xmp.Namespace.Tag.  For example: Xmp.xmp.ModifyDate and Xmp.dc.description.
Because XMP is both extensible and a tree, the tags can contain arrays (XmpSeq) and objects (XmpBag) of arbitrary complexity.  The syntax of XMP tags is handled by (and documented by) the Adobe XMP SDK.

