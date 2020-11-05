Exiv2 supports an EasyAccess API to simplify reading Exif Metadata.

**Overview**

you can use the EasyAccess to search the metadata for the "most likely" key.  For example, to report the value of "WhiteBalance", the following code is sufficient:

```
Exiv2::ExifData::const_iterator metadata = Exiv2::whiteBalance(exifData);
if ( metadata != exifData.end() ) {
    metadata->write(std::cout, &exifData);
}
```

**Declaration and Use**

The EasyAccess API is defined in include/exiv2/easyaccess.hpp.  It is used by the exiv2 command-line program to implement the print summary option `-ps` (which is the default exiv2 option).  For example:

```bash
$ exiv2 http://clanmills.com/Stonehenge.jpg   # or exiv2 -ps http://clanmills.com/Stonehenge.jpg
File name       : http://clanmills.com/Stonehenge.jpg
MIME type       : image/jpeg
Image size      : 6000 x 4000
Thumbnail       : image/jpeg, 10837 Bytes
Camera make     : NIKON CORPORATION
Camera model    : NIKON D5300
Image timestamp : 2015:07:16 15:38:54
File number     : 
Exposure time   : 1/400 s
Aperture        : F10
Exposure bias   : 0 EV
Flash           : No, compulsory
Flash bias      : 
Focal length    : 44.0 mm
Subject distance: 31.62 m
ISO speed       : 200
Exposure mode   : Not defined
Metering mode   : Multi-segment
Macro mode      : 
Image quality   : NORMAL 
White balance   : AUTO        
Copyright       : 
Exif comment    : charset=Ascii Classic View

$
```

This is a small subset of the 168 Exif Metadata tags in Stonehenge.jpg.
```bash
$ exiv2 -pe http://clanmills.com/Stonehenge.jpg | wc
     168     877   11406
$ 
```

The values displayed are "translated" values (human readable).  The difference can be seen using the exiv2 -pv (raw value) and -pt (translated) value:

```
$ exiv2 -pt --grep Photo.WhiteBalance http://clanmills.com/Stonehenge.jpg 
Exif.Photo.WhiteBalance                      Short       1  Auto
$ exiv2 -pv --grep Photo.WhiteBalance http://clanmills.com/Stonehenge.jpg 
0xa403 Photo        WhiteBalance                Short       1  0
$ 
```

This output is generated in src/actions.cpp with the following code:

```bash
$ cd <exiv2dir>
$ grep White src/actions.cpp 
        printTag(exifData, Exiv2::whiteBalance      , _("White balance")                                );
1507 rmills@rmillsmbp:~/gnu/github/exiv2/kmilos $
```

The EasyAccess selector function: `Exiv2::whiteBalance()` is declared in include/exiv2/easyaccess.hpp as follows:

```hpp
    //! Return the white balance setting
    EXIV2API ExifData::const_iterator whiteBalance(const ExifData& ed);
```

And implemented in src/easyaccess.cpp as follows:

```cpp
    ExifData::const_iterator whiteBalance(const ExifData& ed)
    {
        static const char* keys[] = {
            "Exif.CanonSi.WhiteBalance",
            "Exif.Fujifilm.WhiteBalance",
            "Exif.Sigma.WhiteBalance",
            "Exif.Nikon1.WhiteBalance",
            "Exif.Nikon2.WhiteBalance",
            "Exif.Nikon3.WhiteBalance",
            "Exif.Olympus.WhiteBalance",
            "Exif.OlympusCs.WhiteBalance",
            "Exif.Panasonic.WhiteBalance",
            "Exif.MinoltaCs5D.WhiteBalance",
            "Exif.MinoltaCs7D.WhiteBalance",
            "Exif.MinoltaCsNew.WhiteBalance",
            "Exif.MinoltaCsOld.WhiteBalance",
            "Exif.Minolta.WhiteBalance",
            "Exif.Sony1MltCsA100.WhiteBalance",
            "Exif.SonyMinolta.WhiteBalance",
            "Exif.Sony1.WhiteBalance",
            "Exif.Sony2.WhiteBalance",
            "Exif.Sony1.WhiteBalance2",
            "Exif.Sony2.WhiteBalance2",
            "Exif.Casio.WhiteBalance",
            "Exif.Casio2.WhiteBalance",
            "Exif.Casio2.WhiteBalance2",
            "Exif.Photo.WhiteBalance"
        };
        return findMetadatum(ed, keys, EXV_COUNTOF(keys));
    }
```

**Implementation of `Print::printTag()` in src/actions.cpp**

This code is not in the Exiv2 library.  The code is in the exiv2 command-line application and defined in src/actions.cpp:

```cpp
int Print::printTag(const Exiv2::ExifData& exifData,
                    EasyAccessFct easyAccessFct,
                    const std::string& label,
                    EasyAccessFct easyAccessFctFallback /* =NULL*/ ) const
{
    int rc = 0;
    if (!label.empty()) {
        printLabel(label);
    }
    Exiv2::ExifData::const_iterator md = easyAccessFct(exifData);
    if (md != exifData.end()) {
        md->write(std::cout, &exifData);
        rc = 1;
    }
    else if (NULL != easyAccessFctFallback)
    {
        md = easyAccessFctFallback(exifData);
        if (md != exifData.end()) {
            md->write(std::cout, &exifData);
            rc = 1;
        }
    }
    if (!label.empty()) std::cout << std::endl;
    return rc;
} // Print::printTag
``` 

The EasyAccess API searches the array of keys to determine the first metadata key to be used:

```cpp
static const char* keys[] = {
            "Exif.CanonSi.WhiteBalance",
            "Exif.Fujifilm.WhiteBalance",
...
```

**Rationale for EasyAccess API**

To explain how and why this is useful requires an understanding of the structure on Exif metadata in an image.  There are three specifications involved with Exif metadata and they are:

| IFD | Family.Group.Tag | Specification | URL |
|:--  |:--               |:--            |:--  |
| Tiff | Exif.Image.TagName | Adobe Tiff 6.0<br>TIFF/EF | [fdd000073.shtml](https://www.loc.gov/preservation/digital/formats/fdd/fdd000073.shtml)<br>[fdd000022.shtml](https://www.loc.gov/preservation/digital/formats/fdd/fdd000022.shtml) |
| Exif | Exif.Photo.TagName | Exif 2.X | [fdd000146.shtml](https://www.loc.gov/preservation/digital/formats/fdd/fdd000146.shtml)|
| MakerNote | Exif.Canon.TagName<br>Exif.Nikon.TagName<br>etc | Undocumented | [https://exiv2.org/makernote.html](https://exiv2.org/makernote.html) |

When we examine WhiteBalance in Stonehenge.jpg:

```
$ exiv2 --grep  WhiteBalance$ http://clanmills.com/Stonehenge.jpg
Exif.Nikon3.WhiteBalance                     Ascii      13  AUTO        
Exif.Photo.WhiteBalance                      Short       1  Auto
$ 
```

We see that WhiteBalance is stored in the metadata in two locations.  In Exif.Photo.WhiteBalance (in the Exif IFD), it is defined as a Short (16 bit signed integer) with a value of 0.  In Exif.Nikon3.WhiteBalance, Nikon has stored it as a 13 byte ascii string with value "AUTO\0".  The EasyAccess API returns the "Exif.Nikon3.WhiteBalance" value because it is defined in the keys before "Exif.Photo.WhiteBalance".

The order of data presented by Exiv2 is determined by the order in which tags are located in the image.  The order of the MetaData keys in the selector determines the order of "preference" of the selector.  The selector returns the first key located in the ExifData array.

**EasyAccess Selector Functions in Exiv2 v0.27.4 and later**

| a-e | e-f | i-m | m-s | s-w |
|:--  |:--  |:--  |:--  |:--  |
| afPoint<br>apertureValue<br>brightnessValue<br>contrast<br>dateTimeOriginal<br>exposureBiasValue<br>exposureIndex | exposureMode<br>exposureTime<br>flash<br>flashBias<br>flashEnergy<br>fNumber<br>focalLength | imageQuality<br>isoSpeed<br>lensName<br>lightSource<br>macroMode<br>make<br>maxApertureValue |meteringMode<br>model<br>orientation<br>saturation<br>sceneCaptureType<br>sceneMode<br>sensingMethod | serialNumber<br>sharpness<br>shutterSpeedValue<br>subjectArea<br>subjectDistance<br>whiteBalance<br>&nbsp; |

**Comment concerning printTag**

The function `printTag()` has a useful _optional_ fourth parameter to provide a fallback search.  For example:

```cpp
printTag(exifData, Exiv2::exposureTime      , _("Exposure time")    , Exiv2::shutterSpeedValue  );
```

This code reports _**Exposure Time**_ using `Exiv2::exposureTime()` and if not found, tries `Exiv2::shutterSpeedValue()`

**More Documentation**

[https://exiv2.org/doc/easyaccess_8hpp.html](https://exiv2.org/doc/easyaccess_8hpp.html)  
[https://exiv2.org/doc/files.html](https://exiv2.org/doc/files.html)

**Contributors**

| Scope      | Data | Contributor |
|:--         |:--   |:--          |
| Invention  | 2009 |  Carsten Pfeiffer pfeiffer@kde.org |
| Additional selectorFunctions<br>Documentation | 2020 | Milo&scaron; Komar&ccaron;evi&cacute; [@kmilos](https://github.com/kmilos) and Robin Mills [@clanmills](https://github.com/clanmills) |
