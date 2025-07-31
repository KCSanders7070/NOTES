## CRC Video Map (geojson) Loading Specification

### Parsing GeoJSON Files

Each GeoJSON file contains a `"FeatureCollection"` with `features[]`.
You must **pre-process defaults** and then **apply them to each feature** according to CRC rules.

#### Parse & Track `isDefaults` Features First

* Look for features where:

  ```json
  properties: {
    isLineDefaults: true
    // or isSymbolDefaults: true
    // or isTextDefaults: true
  }
  ```

* Track **the last** of each type per file.

  * Follow CRC’s **“last assigned attribute wins”** rule if multiple `isDefaults` of the same type exist. Ref: [Multiple isDefaults Features for Same Type](https://github.com/KSanders7070/Understanding_CRC_GeoJsons/blob/main/Understanding_CRC_GeoJsons.md#multiple-isdefaults-features-for-same-type) section.

**Sample structure to hold defaults *per file*:**

```csharp
class FeatureDefaults {
    public Dictionary<string, object> LineDefaults;
    public Dictionary<string, object> SymbolDefaults;
    public Dictionary<string, object> TextDefaults;
}
```

#### For Each **Non-isDefaults** Feature:

* Check feature type:

  * `"geometry.type"`:

    * `"LineString"` / `"MultiLineString"` → Line Feature
    * `"Point"` with `"text"` → Text Feature
    * `"Point"` without `"text"` → Symbol Feature

* Apply attribute values (`AppliedAttributes`) using this **precedence chain**:

  1. **Feature's own properties** (explicit override)
  2. **File's is\*Defaults values**
  3. **CRC Auto-Assigned values** (fallback only if 1 & 2 are missing) Ref: [CRC-Auto-Assigned Defaults](https://github.com/KSanders7070/Understanding_CRC_GeoJsons/blob/main/Understanding_CRC_GeoJsons.md#keyvalue-assignments---crc-auto-assigned-defaults)

✅ Apply this merge **per feature-type** using rules from CRC spec. Ref: [Feature Types-Associated Key Values](https://github.com/KSanders7070/Understanding_CRC_GeoJsons/blob/main/Understanding_CRC_GeoJsons.md#geojson-feature-types--associated-keysvalues)

---

### Assign Features to Filters (optional)

Each feature should end up stored in one or more filter groups based on the `filters` value(s):

* Look inside each feature’s final merged `properties.filters`:

  * Must be an array of ints.
  * Store this feature under **every** filter it belongs to.

**Recommended structure:**

```csharp
Dictionary<string /* GeoMapId */, Dictionary<int /* filter */, List<ProcessedFeature>>> facilityFeatures;
```

```csharp
class ProcessedFeature {
    public string GeometryType; // "Line", "Symbol", "Text"
    public object Geometry;     // Position[] or MultiLine or Point
    public Dictionary<string, object> AppliedAttributes; // Merged props: style, size, etc.
}
```

---

### Final Output Structure

After all `.geojson` files are parsed:

* For each `GeoMapId` you should have:

  * A dictionary of **FilterIndex → List of features**.
* Every feature includes:

  * Its geometry
  * Its fully resolved `AppliedAttributes`

When the user enables filter `X`, the GUI should:

* Collect all matching features across all `geoMaps` that contain `filters: [X]`.
* Render each with correct visual properties (line `style`, symbol `size`, text `xOffset`, etc).

---

### Final Flow Summary (Pseudocode)

```csharp
foreach (geoMap in facilityConfig.eramConfiguration.geoMaps)
{
    foreach (videoMapId in geoMap.videoMapIds)
    {
        var geojson = LoadGeoJson(videoMapId);
        var defaults = ExtractLastIsDefaults(geojson);
        
        foreach (feature in geojson.features)
        {
            if (IsDefaultsFeature(feature)) continue;
            
            var finalProps = MergeProperties(feature.properties, defaults, feature.type);
            var assignedFilters = finalProps["filters"];
            
            foreach (int filter in assignedFilters)
            {
                facilityFeatures[geoMap.id][filter].Add(new ProcessedFeature
                {
                    GeometryType = ...,
                    Geometry = feature.geometry,
                    AppliedAttributes = finalProps
                });
            }
        }
    }
}
```

---

### After Thought

Consider doing a second pass over the stored GeoMap data to implement "Efficient LineString Handling" like ERAM_2_GEOJSON does by merges consecutive points into a single LineString or MultiLineString feature if the other `AppliedAttributes` are identical.

This may make display of information a bit smoother.


---

### References

* [Understanding GeoJSON File Structure and Rules](https://github.com/KSanders7070/Understanding_CRC_GeoJsons/blob/main/Understanding_CRC_GeoJsons.md)
* [ERAM_2_GEOJSON](https://github.com/Nikolai558/ERAM_2_GEOJSON)
* [ERAM_2_GEOJSON>Output Formats](https://github.com/Nikolai558/ERAM_2_GEOJSON?tab=readme-ov-file#output-format-details)
* [GeoJson by Filter](https://github.com/Nikolai558/ERAM_2_GEOJSON/blob/master/HELPERS/GeoJsonGeneratorByFilters.cs)
* [Geojson by Attribute](https://github.com/Nikolai558/ERAM_2_GEOJSON/blob/master/HELPERS/GeoJsonGeneratorByAttributes.cs)
* [GeoJson All-In-One](https://github.com/Nikolai558/ERAM_2_GEOJSON/blob/master/HELPERS/GeoJsonGeneratorByRaw.cs)
