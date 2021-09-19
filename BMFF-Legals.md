There has been a very vigorous discussion in Team Exiv2 and with the Open Source Community about BMFF support and the possibility of patent enfrigement.  Exiv2 does not accept legal responsibility for enabling and using BMFF support.

![BMFF Support](https://user-images.githubusercontent.com/529982/131215766-adf04e0e-07a7-4df3-820c-e07b82fe4cf2.jpg)

README.md contains the following section:

_**2.19 Support for BMFF files (CR3, HEIF, HEIC, and AVIF)**_

_**Attention is drawn to the possibility that bmff support may be the subject of patent rights. _Exiv2 shall not be held responsible for identifying any or all such patent rights.  Exiv2 shall not be held responsible for the legal consequences of the use of this code_.**_

_Access to the bmff code is guarded in two ways.  Firstly, you have to build the library with the cmake option: `-DEXIV2_ENABLE_BMFF=On`.  Secondly, the application must enable bmff support at run-time by calling the following function._

```cpp
EXIV2API bool enableBMFF(bool enable);
```

_The return value from `enableBMFF()` is true if the library has been build with bmff support (cmake option -DEXIV2_ANABLE_BMFF=On)._

_Applications may wish to provide a preference setting to enable bmff support and thereby place the responsibility for the use of this code with the user of the application._

-------------

**Caution**

The `Exiv2::enableBMFF()` API for v1.00 _(on branch 'main')_ is the same as 0.27.4 (and later), however the build default is `-DEXIV2_ENABLE_BMFF=On` and revealed to the compiler by EXV_ENABLE_BMFF (in exv_conf.h).  At run-time, library initialisation is equivalent to calling `Exiv2::enableBMFF(true);`.

**Summary**

Applications can disable run-time BMFF support by calling `Exiv2::enableBMFF(false);` for any version of Exiv2 (v0.27.4 and later).