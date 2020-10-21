When making changes to the code to support some new feature, we recommend to add a test-image and test-case to the test suite. In that way, we can be sure that the change works in all the supported platforms configured in our Continuous Integration (IC) infrastructure. 

Since images can be very large to keep them in a open source repository, we usually extract the relevant metadata from an image and place it in a new file with extension **.exv**. Then, we can add that file to the repository and use it as it would be an image. To extract these information we can run the following commands:

```bash
# Extract Exif section of metadata into .exv
exiv2 -ee --force ${ImageName}.JPG

# Delete thumbnail from .exv file
exiv2 -dt ${name}.exv

# Delete other sensible information from .exv file
exiv2 \
  -M'del Exif.OlympusEq.SerialNumber' \
  -M'del Exif.OlympusEq.InternalSerialNumber' \
  -M'del Exif.OlympusEq.LensSerialNumber' \
  -M'del Exif.OlympusEq.ExtenderSerialNumber' \
  -M'del Exif.OlympusEq.FlashSerialNumber' \
  ${name}.exv

# Rename file to some meaningful name for your test case
mv ${name}.exv olympus-m.zuiko-17mm-f1.2-pro.exv
```