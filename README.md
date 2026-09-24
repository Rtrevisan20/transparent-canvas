# transparent-canvas

This is a Delphi VCL / Windows project for drawing semi-transparent alphablended graphics. It provides a class similar to TCanvas.

Project home page: http://parnassus.co/open-source/ttransparentcanvas/

Note: this project used to be hosted on Google Code (code.google.com/p/transparent-canvas) and was moved on 2015-03-12.

## Delphi and Lazarus (FPC) support

The component is **Windows-only** (it uses GDI, `AlphaBlend` from msimg32.dll and
`DrawThemeTextEx` from uxtheme.dll), but the source compiles and runs on **both**
Delphi (VCL) and **Lazarus / Free Pascal (LCL)** on Windows. `TransparentCanvas.pas`
uses `{$ifdef FPC}` guards where the two libraries diverge.

### Using it from Lazarus / FPC

Add `TransparentCanvas.pas` to your LCL project (or a package). Because the unit is
written in Delphi-compatible Pascal, compile it with `-Mdelphi` (which the Lazarus IDE
uses by default for Delphi-mode projects) and make sure these units are on the unit
search path:

- `lcl/units/<target>` (e.g. `C:\lazarus\lcl\units\x86_64-win64`)
- `lcl/units/<target>/win32`
- `components/lazutils/lib/<target>` (for `LazFileUtils`, used by `LCLProc`)

Command line example:

```
fpc -Mdelphi -FuC:\lazarus\lcl\units\x86_64-win64 ^
    -FuC:\lazarus\lcl\units\x86_64-win64\win32 ^
    -FuC:\lazarus\components\lazutils\lib\x86_64-win64 ^
    TransparentCanvas.pas
```

### Notes on Lazarus behaviour

- `SaveToFile` writes the 32-bit BMP directly from the component's own DIB on FPC,
  because LCL `TBitmap` does not sync raw GDI drawing done through its
  `Canvas.Handle` back to its pixel store.
- `DrawTo` / `Draw` onto the canvas of a **control** (form, paint box) works as on
  Delphi. Drawing onto a `TBitmap.Canvas` and then using that bitmap will not reflect
  the drawing in Lazarus (same LCL limitation); composite to a control canvas or use
  `SaveToFile` instead.
- `GlowTextOut` requires `DrawThemeTextEx`, which is only available on
  Windows Vista and later. On older systems `CanDrawGlowText` returns `False` and
  `GlowTextOut` raises an exception.
- The demo programs in `demo/` are VCL- (Delphi-) only and have not been ported to LCL.

## Original demos

The `demo` folder contains Delphi VCL demo projects demonstrating the component's
features.