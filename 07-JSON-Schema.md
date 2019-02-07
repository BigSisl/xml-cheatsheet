# JSON Schema

## Restriktionsregeln
```
// Zahlen
maximum, exclusiveMaximum, minimum,
exclusiveMinimum, multipleOf

// Strings
maxLength, minLength, pattern

// Arrays
maxItems, minItems, uniqueItems,
items, additionalItems, contains

// Objekte
maxProperties, minProperties,
required, propertyNames,
properties, additionalProperties
```

## Zus. Items und Properties verhindern
```json
"additionalItems": false,
"additionalProperties": false
```

## Referenzen
```json
"$ref": "#/definitions/person",
"$ref": "../schema.json#/definitions/person",
"$ref": "http://a.ch/s.json#/definitions/person",
```

## Definition erweitern
```json
"type": "object",
"allOf": [
  { "$ref": "#/definitions/person" },
  { "required": ["gender"], "properties": {
    "gender": { "enum": ["female", "male"] }
  }}
]
```

## Beispiel
```json
{
  "$id": "http://ex.com/movie.json",
  "title": "James Bond Movie Casting",
  "definitions": {
    "person": {
      "type": "object",
      "properties": {
        "name": { "type": "string" },
        "gender": {
          "enum": ["female", "male"]
        }
      },
      "required": ["name", "gender"],
      "additionalProperties": {
        "type": "string"
      }
    }
  },
  "type": "object",
  "required": ["id", "title"],
  "properties": {
    "id": {
      "type": "number",
      "minimum": 10,
      "maximum": 999
    },
    "title": {
      "type": "string",
      "minLength": 4,
      "maxLength": 25,
      "pattern": "007"
    },
    "director": {
      "const": "Marc Forster"
    },
    "bond": {
      "type": "array",
      "items": {
        "oneOf": [
          { "$ref": "#/definitions/person" },
          { "type": "string" }
        ]
      },
      "uniqueItems": true,
      "minItems": 6,
      "maxItems": 6
    },
    "ratings": {
      "type": "array",
      "items": {
        "type": "array",
        "items": [
          { "type": "integer", "minimum": 0 },
          { "type": "integer", "maximum": 5 },
          { "type": "string" }
        ],
        "additionalItems": false
      }
    },
    "scene": {
      "type": "object",
      "properties": {
        "description": { "type": "string" },
        "stuntman": { "type": "boolean" },
        "type": {
          "enum": ["kiss", "action"]
        }
      },
      "additionalProperties": {
        "type": "string"
      }
    },
    "year": {
      "oneOf": [
        {
          "type": "integer",
          "minimum": 1900
        }, {
          "type": "string",
          "enum": ["unknown", "TBA"]
        }
      ]
    },
    "duration": {
      "type": "integer",
      "anyOf": [
        { "multiplyOf": 20},
        { "multiplyOf": 30}
      ]
    }
  }
}
```