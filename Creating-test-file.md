When making changes to the C++ code, we recommend adding a test-image and test-case to the test suite. In that way, we can be sure that the change works in every supported platforms configured in our Continuous Integration (CI) infrastructure. 

Since images can be very large, we usually extract the relevant metadata from an image and place it in a new file with extension **.exv**. Then, we can add that file to the repository and use it for test purposes. You can extract a **.exv** from your image with the following command:

```bash
# Extract Exif section of metadata into .exv
exiv2 -ea --force foo.jpg

# Delete thumbnail from .exv file
exiv2 -dt foo.exv

# Delete other sensitive information from .exv file
exiv2 \
  -M'del Exif.OlympusEq.SerialNumber' \
  -M'del Exif.OlympusEq.InternalSerialNumber' \
  -M'del Exif.OlympusEq.LensSerialNumber' \
  -M'del Exif.OlympusEq.ExtenderSerialNumber' \
  -M'del Exif.OlympusEq.FlashSerialNumber' \
  foo.exv

# Rename file to some meaningful name for your test case
mv foo.exv olympus-m.zuiko-17mm-f1.2-pro.exv
```