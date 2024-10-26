# YAML bindings

## How Bindings Work
1. **Parsing Phase**
   - YAML front matter is parsed according to the specified version
   - Bindings are validated against schema definitions
   - Configuration values are transformed into rainlang components

2. **Composition Phase**
   - Bindings are resolved in dependency order
   - Values are injected into rainlang fragments
   - Final document is assembled for deployment
  
## Requirements
* Follow [YAML 1.2 Specification](https://yaml.org/spec/)
* Adhere to [dotrain YAML Schema](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md)
* Maintain proper indentation and structure
* Use consistent data types

## Best Practices

* Comment complex bindings
* Explain non-obvious constraints
* Document dependencies between bindings
* Include examples for reference
* Validate bindings before deployment
* Test with different version combinations
* Verify binding resolution
* Check for circular dependencies

## Common Pitfalls

* Mixing incompatible versions
* Undocumented version dependencies
* Missing version specifications
* Unresolved references
* Type mismatches
* Invalid value formats
* Circular dependencies
