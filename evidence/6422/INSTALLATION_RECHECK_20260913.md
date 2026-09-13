<!-- This file was generated with the assistance of an AI coding tool. -->
# Windows installation recheck for #6422 / PR #9472

## Scope and measured results

Rechecked on 2026-09-13 UTC with Blender 4.5.4 LTS, build b3efe983cc58.
The tested archives were kept separate and installed into two new isolated profiles.
No production Blender profile was modified.

| Check | Result |
| --- | --- |
| Bonsai 0.9.0-alpha260909, Windows py311, fresh profile | Installation/registration failed, exit 1: `No schema named IFC4` |
| Embedded 0.9.0 wheel inventory | No `ifcopenshell_parse_schema_ifc*.dll` files present |
| Bonsai 0.8.5-post1, Windows py311, separate fresh profile | Installed and enabled, exit 0 |
| Separate process loading that stable profile | Exit 0: `INSTALL_PROBE 4.5.4 LTS 0.8.5-post1 IFC4 True` |
| PR #9472 parser regression suite at f1c0eaa1792dfb78a0d8bb5791a80f0cac00c240 | 17 unittest tests passed, exit 0 |

In the installation probe, `IFC4` is the result of `ifcopenshell.schema_by_name("IFC4").name()`.
`True` means `bpy.types.Scene.DocProperties` exists after Bonsai registration.
This checks native schema availability and add-on registration, not a full geometry or drawing workflow.

## Root cause, not a user permissions repair

The failure reproduces without an old installation. The missing runtime schema libraries are consistent with [#9423's corrected diagnosis](https://github.com/IfcOpenShell/IfcOpenShell/issues/9423#issuecomment-5520207727).
The `Failed to remove` messages occur during rollback after registration fails; reinstalling or deleting unrelated extension dependencies cannot supply absent DLLs.
