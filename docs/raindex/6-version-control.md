# Version control in dotrain


- The dotrain tooling is a versioned dependency that comes with that specified version of the raindex app. Within the context of using raindex, it is imperative that every rain document is tied to a specific raindex app release, so that it can be parsed and composed by the tooling associated with that release.
- Versioning documents prevents errors or misinterpretations that might arise from version mismatches which result in erreneous bytecodes being generated for the document.
- Versioning enables tooling to be iteratively improved without disrupting existing `.rain` files, with older documents remaining functional with the specified versions they were designed for.
- Versioning provides a reference point for audit trial of the compositions and any modifications to the document to be made in the future. 
- You can easily access the raindex version when you install and open the app. Navigate to the `New Order` section tab and the editor will prompt the raindex app version.

<img src="/img/raindex_version.png" />  

### Raindex version specification
```yaml
raindex-version: db14c87f012a76980661802ff424371d6e84552e
```
The `raindex-version` field is mandatory:
* Ensures compatibility between your configuration and the tooling by doing simulation runs for the scenario and asserting the results for the written rainlang.
* Determines which parser version processes your fragments, generally this is the deployer contract that is present in the `deployers` section. 
* Maintains consistency across deployments to prevents unexpected behavior from version mismatches.

### Version management best practices
* Always specify an explicit version within the rain document.
* Document version changes in your repository by partioning different versioned documents within different folders so that there is clear distinction between them.
* Test configurations when upgrading versions and the settings.

