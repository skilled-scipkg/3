# mumax3 source map: Inputs and Modeling

Use this only after `doc_map.md`.

## Fast queries
- `rg -n "SetMesh|SetGridSize|SetCellSize|SetPBC|SetGeom|EdgeSmooth|InitGeomFromOVF" engine`
- `rg -n "DefRegion|RedefRegion|SetRegion\\(|FunctionFromDatafile|AddFieldTerm|AddEdensTerm|LoadFile" engine`

## Function-level entry points
- `engine/mesh.go`: mesh, cell size, PBC, and resize side effects.
- `engine/geom.go`: geometry rasterization, `EdgeSmooth`, and geometry import from OVF.
- `engine/shape.go`: primitive shapes, boolean composition, transforms, and image-based shapes.
- `engine/regions.go`: region definition, region replacement, per-cell region access, and region import.
- `engine/parameter.go`: region-wise parameters and time-dependent parameter updates.
- `engine/magnetization.go`: initial states, region-specific magnetization, and `m.LoadFile(...)`.
- `engine/excitation.go`: vector excitations such as `B_ext` and `J`, including mask-based extra terms.
- `engine/scalar_excitation.go`: scalar excitation handling when the driven quantity is scalar-valued.
- `engine/functionfromfile.go`: CSV interpolation for time-dependent inputs.
- `engine/customfield.go`: custom field terms, custom energies, and quantity algebra.

## High-value behavior checks
- `mumax3 -vet test/regions.mx3`
- `mumax3 -vet test/functionfromfile.mx3`
- `mumax3 -vet test/initgeomfromovf.mx3`
- `mumax3 test/geom2d.mx3`
