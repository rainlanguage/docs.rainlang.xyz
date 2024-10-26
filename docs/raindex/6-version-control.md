# Version control in dotrain

[insert how to add the raindex-version, is it automatic?]

### Raindex version specification
```yaml
raindex-version: db14c87f012a76980661802ff424371d6e84552e
```
The `raindex-version` field is mandatory:
* Ensures compatibility between your configuration and the tooling
* Determines which parser version processes your fragments
* Maintains consistency across deployments
* Prevents unexpected behavior from version mismatches

### Version management best practices
* Always specify an explicit version
* Document version changes in your repository
* Test configurations when upgrading versions
* Maintain version consistency across related configurations
