# Good and bad pyd files from mingw-w64 compilation

Both of these files are DLLs for Python import (hence the `.pyd` suffix).

In fact both correspond to the `_distance_wrap....pyd` file from a Scipy build,
using Meson.

The build for the `.pyd` files uses MinGW-w64 to link other MinGW-w64 compiled
code against a static MSVC-generated `.lib` from Numpy, called `npymath.lib`.

`good.pyd` imports correctly.  It linked against an `npymath.lib` compiled with
MSVC toolchain version 141, corresponding to Visual Studio 2017.

`bad.pyd` fails on import, with error of form:

```
C:\repos\scipy\installdir\Lib\site-packages\scipy\spatial\_distance_wrap.cp39-win_amd64.pyd
   Error 193: C:\repos\scipy\installdir\Lib\site-packages\scipy\spatial\_distance_wrap.cp39-win_amd64.pyd
is not a valid Win32 application.
```

(This error comes from importing the `.pyd` file with its original name).

`bad.pyd` linked against `npymath.lib` compiled with MSVC toolchain version
142, corresponding to Visual Studio 2019.

## Outputs

`llvm-ro-{good,bad}.txt` are the outputs from `llvm-readobj.exe --all` for each
`.pyd` file.  Similarly for `{good,bad}_npymath.lib`.

`objdump_x_{good,bad}.txt` are the outputs from `objdump -x`.

`dlldiag_{good,bad}.txt` are the outputs from `dlldiag trace`, from an
Administrator shell, after installing the WinDbg tools.

<https://github.com/adamrehn/dll-diagnostics>
