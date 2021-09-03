The TagName syntax is a 3 part tuple in the syntax _**Family.Group.Key**_.  For example: Exif.Image.Make.  There are three families: Exif, Iptc and Xmp.

The **Exif** tags are in a hierarchy.  The _**Group**_ indicates the IFD in which the tag was found.  At the top are tags defined in the TIFF/EP specification and are named: Exif.Image.Key.

Exif defines tags in the Image IFD called ExifTag (0x8769) and GPSTag (0x8825) which are both IFDs.  The tags in those IFDs are in Group _**Photo**_ and _**GPSInfo**_, so the tags are named: Exif.Photo.Key and Exif.GPSInfo.Key.

An IFD is an array of tags.  Multiple arrays can be chained.  In the Exif IFD, the second array normally contains thumbnail images.  The tags in this array are named: Exif.Thumbnail.Key.

Exif defines a tag called MakerNote (0x927c) which introduces the camera manufacturer's data.  This is normally a short header followed by an IFD.  Exif.Nikon.Key are tags in the Nikon MakerNote.  For example, Exif.Nikon3.SerialNumber is an ascii tag in the Nikon MakerNote.  Most manufacturers have different MakerNote designs for different camera models and identified by different families.  Nikon have used 3 different MakerNote designs which belong to the families Nikon1, Nikon2 and Nikon3.

In addition to the Exif formatted tags, the MakerNote usually contains binary structures.  For example: Exif.NikonPc.Key are the fields in the _**PictureControl**_ structure which is stored in the PictureControl tag in the Nikon MakerNote.  For example: Exif.NikonPc.Sharpness and Exif.NikonPc.Contrast.

![ExifTagNames](https://user-images.githubusercontent.com/529982/131121319-5d4dfcb6-2cd2-49ce-8216-73a311cb61e1.png)

Exif Metadata is stored in a Tiff formatted sub-file of the image.  The arrangements for the storage of this sub-file are defined by the file format.  The Exif metadata is independent of the file format.  The following drawing illustrates the design of a Tiff file.

![Tiff File Layout](https://user-images.githubusercontent.com/529982/131124453-afc30d02-c845-42a3-9a84-299b569b712c.png)

**IPTC** tags are simpler.  IPTC metadata is two level structure.  There are 9 "sections" such as Envelope and Application2 for which the specification defines the keys.  The syntax Family.Group.Key maps to Iptc.Section.Key.  For example:  Iptc.Envelope.ModelVersion and Iptc.Application2.Caption.

XMP is represented in XML and therefore a heirarchical tree.  The syntax Family.Group.Key maps to Xmp.Namespace.Key.  For example: Xmp.xmp.ModifyDate and Xmp.dc.description.  Because XMP is both extensible and a tree, the tags can contain arrays (XmpSeq), objects (XmpBag), structs (XmpStruct) of arbitrary complexity.  The syntax of XMP keys is handled by (and documented by) the Adobe XMP SDK.

