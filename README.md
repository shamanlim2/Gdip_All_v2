# Gdip_All_v2
Gdip_All for AutoHotkey v2
This is a modified version of the Gdip_All library, specifically optimized for AutoHotkey v2.

## ⚠️ Work In Progress (WIP)
This repository is a migration of the Gdip library to **AutoHotkey v2**. While many functions are working, this is still an experimental version. We are currently fixing legacy v1 syntax patterns that cause instability in v2.

### Known Issues & To-Do List
The following patterns are being actively scanned and corrected throughout the library:

1.  **Incorrect Buffer Referencing**: 
    * **Problem**: Many `DllCall` instances still use `&BufferObject` for `Ptr` types, which passes the address of the object itself rather than the memory buffer.
    * **Fix**: Update to pass `BufferObject` or `BufferObject.Ptr` directly.
    * **Affected**: `UpdateLayeredWindow`, `MDMF_GetInfo`, and various struct-related functions.

2.  **Missing VarRef (`&`) in DllCalls**:
    * **Problem**: v2 requires the address-of operator `&` for output parameters (e.g., `"UInt*", OutputVar` must be `"UInt*", &OutputVar`).
    * **Affected**: `Gdip_BitmapFromBase64`, `Gdip_GetImageDimension`, and functions using `Ptr*` or `UInt*`.

3.  **Ambiguous Return Types (Handle Issues)**:
    * **Problem**: Many `DllCall`s lack an explicit return type, defaulting to `Int`. In 64-bit environments, this can cause handles (HDC, HWND) to be truncated or returned as negative values.
    * **Fix**: Explicitly define `"UPtr"` as the return type for all handle-generating functions.

4.  **Legacy Pointer Variable Usage**:
    * **Problem**: Use of `Ptr := "UPtr"` as a variable instead of direct strings in `DllCall`.
    * **Fix**: Standardizing all `DllCall` types to explicit strings for better performance and clarity.

5.  **StrGet/StrPut Adjustments**:
    * **Problem**: Incorrect offset calculations when used with Buffer objects (e.g., `StrGet(&Buffer + Offset)`).
    * **Fix**: Update to `StrGet(Buffer.Ptr + Offset)`.

---
*Contributions and Pull Requests are welcome to help finalize the v2 migration!*
