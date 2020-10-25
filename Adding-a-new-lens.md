1. Before changing the C++ code to recognise a lens, please consider using the **Configuration File** as documented in the man page: [https://exiv2.org/manpage.html](https://exiv2.org/manpage.html)

2. We will not accept a change to the C++ code without a test image.  The procedure to create a test file is documented here: [https://github.com/Exiv2/exiv2/wiki/Create-test-file](https://github.com/Exiv2/exiv2/wiki/Create-test-file)

3. Add a python script to test your change using your test image. Here's an example from a user: [https://github.com/Exiv2/exiv2/issues/1368.](https://github.com/Exiv2/exiv2/issues/1368). I explained the test harness to him here: https://github.com/Exiv2/exiv2/issues/1368#issuecomment-711407187

4. Run the test suite before submitting your PR.  This is documented in README.md.

5. Until we release Exiv2 0.28, we will have two development branches - '0.27-maintenance' and 'master'. Please focus your PR on 0.27-maintenance and it will be ported to 'master'.  Once Exiv2 v0.28 ships (mid 2021), please focus your PR on 'master'.  It will not be back-ported to 0.27-maintenance.

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
