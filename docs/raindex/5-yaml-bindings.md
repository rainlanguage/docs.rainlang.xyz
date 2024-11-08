# YAML bindings

## How Bindings Work
1. **Parsing Phase**
   - YAML front matter is parsed by the tooling available with the specified version of the raindex app.
   - Bindings are validated against schema definitions (refer official spec linked below).
2. **Composition Phase**
   - Bindings are resolved in dependency order.
   - Values are injected into rainlang fragments.
   - Final document is assembled for deployment.
  
## Requirements
* Follow [YAML 1.2 Specification](https://yaml.org/spec/)
* Adhere to [dotrain YAML Schema](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md)
* Maintain proper indentation and structure that can be merged to support multiple files. 

## Best Practices
* Comment complex bindings and remove all the unnecessary bindings.
* Prefer structures that could be merged to support cascading/overriding of multiple files.
* Avoid needless depth in hierarchies, prefer flat, except where it would imply excessive repetition.
* Include binding examples wherever necessary.
* Validate all the bindings before deployment, resolved bindings can be previewed in the `Rainlang` tab of the raindex editor.

## Common Pitfalls
The composition tooling we have within raindex editor prompts in case of any inconsistencies and errors within the document. However, the following points should be checked before submitting the document on-chain.
* Ensure the document is compatible with the proposed raindex app and settings (deployer, orderbook contracts etc).
* Ensure all the binding values are correct; the app checks for type inconsistencies but the values need to be verified. E.g.: Buy price binding is incorrectly set to `100` instead of `50`.
* Missing version specifications
* Unresolved bindings within the document.
* Type mismatches 
* Invalid value formats
* Indentation errors due to increased verbosity in case of large documents.
