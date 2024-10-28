# Version control in dotrain


- The dotrain tooling is a versioned dependency that comes with that specific version of the raindex app. It is imperative that every rain document is tied to a specific raindex app release and the tooling that comes with it. You can easily access the raindex version when you install and open the app. Navigate to the `New Order` section tab and the editor will prompt the raindex app version.

<img src="/img/raindex_version.png" />  

- Alternatively you can navigate to the [release page](https://github.com/rainlanguage/rain.orderbook/releases), select the release you want to download and the raindex-version is the github sha commit hash assoicated with the release.
- Eg : The raindex version associate with [this raindex app](https://github.com/rainlanguage/rain.orderbook/releases/tag/app-v0.0.0-db14c87f012a76980661802ff424371d6e84552e) is `db14c87f012a76980661802ff424371d6e84552e`.

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

