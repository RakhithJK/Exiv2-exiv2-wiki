1. Before changing the C++ code to recognise a lens, please consider using the **Configuration File** as documented in the man page: [https://exiv2.org/manpage.html](https://exiv2.org/manpage.html)

2. The code required to recognise a new lens is different for each manufacturer.  I've documented the approach for Nikon lenses below.  If you update the procedure for another manufacturers, please let me know and I will update this wiki page. 

3. We will not accept a change to the C++ code without a test image.  The procedure to create a test file is documented here: [https://github.com/Exiv2/exiv2/wiki/Create-test-file](https://github.com/Exiv2/exiv2/wiki/Create-test-file)

4. Add a python script to test your change using your test image. Here's an example from a user: [https://github.com/Exiv2/exiv2/issues/1368.](https://github.com/Exiv2/exiv2/issues/1368). I explained the test harness to him here: https://github.com/Exiv2/exiv2/issues/1368#issuecomment-711407187

5. Run the test suite before submitting your PR.  This is documented in README.md.

6. Until we release Exiv2 0.28, we will have two development branches - '0.27-maintenance' and 'master'. Please focus your PR on 0.27-maintenance and it will be ported to 'master'.  Once Exiv2 v0.28 ships (mid 2021), please focus your PR on 'master'.  It will not be back-ported to 0.27-maintenance.

**Adding a new Nikon Lens**

The code for nikon lens's is in src/nikonmn_int.cpp:

```cpp
static const struct FMntLens {unsigned char lid,stps,focs,focl,aps,apl,lfw, ltype, tcinfo, dblid, mid; const char *manuf, *lnumber, *lensname;}
fmountlens[] = {
....
// https://github.com/Exiv2/exiv2/issues/743
{0xc9,0x48,0x37,0x5c,0x24,0x24,0x4b,0x4e,0x01,0x00,0x00, "Sigma", "", "24-70mm F2.8 DG OS HSM Art"},
//  https://github.com/Exiv2/exiv2/issues/598 , https://github.com/Exiv2/exiv2/pull/891
{0xCF,0x47,0x5C,0x8E,0x31,0x3D,0xDF,0x0E,0x00,0x00,0x00, "Tamron", "A030", "SP 70-300mm F/4-5.6 Di VC USD"},
//
{0xf4,0x4c,0x7c,0x7c,0x2c,0x2c,0x4b,0x02,0x00,0x00,0x00, "Sigma", "", "APO Macro 180mm F3.5 EX DG HSM"},
// https://github.com/Exiv2/exiv2/issues/1078
{0x80,0x48,0x1C,0x29,0x24,0x24,0x7A,0x06,0x00,0x00,0x00, "Tokina", "", "atx-i 11-16mm F2.8 CF"},
// https://github.com/Exiv2/exiv2/issues/1069
{0xc8,0x54,0x44,0x44,0x0d,0x0d,0xdf,0x46,0x00,0x00,0x00, "Tamron", "F045", "SP 35mm f/1.4 Di USD"},
// https://github.com/Exiv2/exiv2/pull/1105
{0xCB,0x3C,0x2B,0x44,0x24,0x31,0xDF,0x46,0x00,0x00,0x00, "Tamron", "A037", "17-35mm F/2.8-4 Di OSD"},
// https://github.com/Exiv2/exiv2/issues/1208
{0xC8,0x54,0x62,0x62,0x0C,0x0C,0x4B,0x46,0x00,0x00,0x00, "Sigma", "321550", "85mm F1.4 DG HSM | A"},
// Always leave this at the end!
{0,0,0,0,0,0,0,0,0,0,0, NULL, NULL, NULL}
};
```
There are 11 metadata items/lens that must match your lens.  You can obtain those values from a photo taken with the lens.  This is discussed in the issues referenced in the code.  We also require a (simple) python test file when we update C++ lens recognition code.

**Summary of new lens PR**

Here is a typical python test.  See [https://github.com/Exiv2/exiv2/pull/992](https://github.com/Exiv2/exiv2/pull/992).  The user provided a test python file tests/bugfixes/github/test_pr_992.py

```python
# -*- coding: utf-8 -*-

import system_tests

class NikonSigmaLens_APO_MACRO_180_F35_EX_DG_HSM(metaclass=system_tests.CaseMeta):
    url = "https://github.com/Exiv2/exiv2/pull/992"
    
    filename = "$data_path/Sigma_APO_MACRO_180_F3.5_EX_DG_HSM.exv"
    commands = ["$exiv2 -pa --grep lensid/i $filename"]
    stderr = [""]
    stdout = [""
        """Exif.NikonLd3.LensIDNumber                   Byte        1  Sigma APO Macro 180mm F3.5 EX DG HSM
"""
]
    retval = [0]
```

The test file is test/data/Sigma\_APO\_MACRO\_180\_F3.5\_EX\_DG\_HSM.exv.  The tests executes the program `exiv2 -pa --grep lensid/i foo.exv` and compares the output to stdout.  That's it.

In this case, there is only one exiv2 command being executed on a single file.  Most tests are more involved.  You'll notice that the variables: _commands, stderr, stdout and retval_ are arrays to make it easy to execute several commands in a single test.  The most common program to run is exiv2, however other programs from build/bin or system commands can be invoked.  You may not pipe data between commands, however you can redefine the stdin/stdout filter _decode_output_ if required.
