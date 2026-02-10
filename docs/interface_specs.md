# Interface Specs (Draft)

This document defines two lightweight interface specs for creating a generic, adaptable GIS micro-analysis tool while preserving the current project architecture. It is intentionally framework-agnostic and can be applied to the existing single-page HTML/JS app.

---

# 1) Module Interface Spec (Analysis Modules)

## 1.1 Purpose
Define a consistent contract for any analysis module (e.g., Population, Employment, LBAR, Essential Services) so modules can be added/removed without altering the core geometry, UI shell, or data ingestion utilities.

## 1.2 Module Contract
Each module is a plain object with the following required fields:

```json
{
  "id": "string",
  "name": "string",
  "description": "string",
  "version": "string",
  "enabledByDefault": true,
  "geometryScope": "union" | "per-station" | "custom",
  "requiredInputs": [
    {
      "id": "string",
      "label": "string",
      "type": "file" | "api" | "manual" | "computed",
      "format": "csv" | "geojson" | "json" | "none",
      "required": true,
      "mapping": {
        "fields": [
          { "name": "string", "type": "string" | "number" | "geo", "required": true }
        ]
      }
    }
  ],
  "compute": "function(context) => ModuleResult",
  "outputMetrics": [
    {
      "id": "string",
      "label": "string",
      "unit": "string",
      "type": "number" | "percent" | "ratio" | "text",
      "precision": 0
    }
  ],
  "outputLayers": [
    {
      "id": "string",
      "label": "string",
      "type": "fill" | "line" | "circle" | "symbol",
      "source": "geojson"
    }
  ],
  "notes": ["string"]
}
```

## 1.3 Context Object
The `compute(context)` function receives a context object built by the core engine:

```json
{
  "geometry": {
    "stations": "FeatureCollection<Point>",
    "buffers": "FeatureCollection<Polygon>",
    "union": "Feature<Polygon|MultiPolygon>"
  },
  "inputs": {
    "files": { "[inputId]": "ParsedData" },
    "api": { "[inputId]": "ApiResponse" },
    "manual": { "[inputId]": "UserValue" }
  },
  "helpers": {
    "areaWeight": "function",
    "pointInPolygon": "function",
    "intersect": "function",
    "toGeoId": "function",
    "log": "function"
  },
  "config": {
    "bufferMiles": 0.5,
    "crs": "EPSG:4326",
    "profileId": "string"
  }
}
```

## 1.4 ModuleResult Output
`compute(context)` must return a consistent result object:

```json
{
  "metrics": [
    {
      "id": "string",
      "value": 123,
      "unit": "string",
      "notes": "string"
    }
  ],
  "layers": [
    {
      "id": "string",
      "geojson": "FeatureCollection"
    }
  ],
  "warnings": ["string"],
  "errors": ["string"]
}
```

## 1.5 Rules
- Modules **must not** mutate shared state directly.
- Modules **must** depend on `context.geometry.union` unless `geometryScope` explicitly states `per-station` or `custom`.
- Modules **should** return empty `metrics`/`layers` with a warning if required inputs are missing.
- Modules **must** be deterministic for a given `context`.

---

# 2) Tool Profile Spec (Configuration Profiles)

## 2.1 Purpose
Define a single configuration file that turns the core engine into a bespoke GIS micro-analysis tool without code changes. A profile declares which modules are active, their input mappings, buffer settings, rating rules, and UI labels.

## 2.2 Profile Contract
A tool profile is a JSON document:

```json
{
  "profileId": "string",
  "name": "string",
  "description": "string",
  "version": "string",
  "defaults": {
    "bufferMiles": 0.5,
    "stationBufferMiles": 0.5,
    "stationServiceMiles": 1.0,
    "geometryPrecision": 6
  },
  "modules": [
    {
      "moduleId": "string",
      "enabled": true,
      "inputMappings": {
        "[inputId]": {
          "source": "file" | "api" | "manual" | "computed",
          "format": "csv" | "geojson" | "json",
          "fieldMap": {
            "[fieldName]": "columnName"
          }
        }
      }
    }
  ],
  "ratingRules": [
    {
      "metricId": "string",
      "scale": [
        { "label": "High", "min": 100, "max": null },
        { "label": "Medium", "min": 50, "max": 99.999 },
        { "label": "Low", "min": null, "max": 49.999 }
      ],
      "rounding": 2,
      "notes": "string"
    }
  ],
  "ui": {
    "title": "string",
    "subtitle": "string",
    "disclaimer": "string",
    "moduleOrder": ["moduleId"],
    "labels": {
      "stations": "Stations",
      "buffers": "Buffers",
      "union": "Station Area"
    }
  }
}
```

## 2.3 Validation Rules
- `profileId` must be unique across all profiles.
- `modules[].moduleId` must match a registered module `id`.
- `ratingRules[].metricId` must exist in at least one active module output.
- A profile should be usable with **no code edits** when added to the app.

## 2.4 Example Use Cases
- **Small Starts Profile**: Uses ACS, LODES, LBAR, CRE, Essential Services with 0.5-mile buffers and FTA breakpoints.
- **Corridor Screening Profile**: Uses population density, job access, and a custom service count module with simplified rating thresholds.
- **Equity Focus Profile**: Emphasizes CRE + essential services, with optional employment and no LBAR.

---

# 3) Next Steps (Optional)
1. Convert existing modules into isolated `Module` objects without altering outputs.
2. Create one profile file (`small-starts.json`) that mirrors the current behavior.
3. Add a simple profile selector that loads module configs and rating rules at runtime.
tool_profile_template.jsontool_profile_template.json
New
+89-0
{
  "profileId": "template-profile",
  "name": "Template Profile",
  "description": "Starter template for configuring a bespoke GIS micro-analysis tool.",
  "version": "0.1.0",
  "defaults": {
    "bufferMiles": 0.5,
    "stationBufferMiles": 0.5,
    "stationServiceMiles": 1.0,
    "geometryPrecision": 6
  },
  "modules": [
    {
      "moduleId": "population",
      "enabled": true,
      "inputMappings": {
        "acs": {
          "source": "api",
          "format": "json",
          "fieldMap": {
            "population": "B01003_001E",
            "households": "B11001_001E"
          }
        }
      }
    },
    {
      "moduleId": "employment",
      "enabled": false,
      "inputMappings": {
        "lodes": {
          "source": "file",
          "format": "csv",
          "fieldMap": {
            "geoid": "w_geocode",
            "jobs": "C000"
          }
        }
      }
    },
    {
      "moduleId": "essential-services",
      "enabled": true,
      "inputMappings": {
        "services": {
          "source": "file",
          "format": "geojson",
          "fieldMap": {
            "name": "name",
            "type": "type"
          }
        }
      }
    }
  ],
  "ratingRules": [
    {
      "metricId": "population-density",
      "scale": [
        { "label": "High", "min": 8000, "max": null },
        { "label": "Medium", "min": 4000, "max": 7999.999 },
        { "label": "Low", "min": null, "max": 3999.999 }
      ],
      "rounding": 0,
      "notes": "Replace with project-specific breakpoints."
    },
    {
      "metricId": "essential-services-average",
      "scale": [
        { "label": "High", "min": 10, "max": null },
        { "label": "Medium", "min": 5, "max": 9.999 },
        { "label": "Low", "min": null, "max": 4.999 }
      ],
      "rounding": 1,
      "notes": "Adjust based on local expectations."
    }
  ],
  "ui": {
    "title": "Generic GIS Micro-Analysis Tool",
    "subtitle": "Configurable screening tool",
    "disclaimer": "Planning-grade estimates only.",
    "moduleOrder": ["population", "essential-services", "employment"],
    "labels": {
      "stations": "Stations",
      "buffers": "Buffers",
      "union": "Station Area"
    }
  }
}
