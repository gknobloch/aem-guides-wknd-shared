# Adjusting & releasing the package for Summit 2023

The package has been adjusted to be able to run Summit 2023 Lab 713
See also https://github.com/Adobe-Marketing-Cloud/Summit-2023-L713

## Adjust pom files

After making new adjustments to the package content, adjust the 3 pom files by incrementing the build number
* `pom.xml`
* `all/pom.xml`
* `ui.content/pom.xml`

Commit changes and push to the fork.

## Deploying a new version

Start running `maven clean install` from the root directory.

Set the `PACKAGE_VERSION` environment variable version to the target release version.
```
export PACKAGE_VERSION=2.1.3-S23-L713-R001
```

### Deploy parent pom

Run the following command from the root directory to deploy the parent pom
```
mvn -Partifactory-corp deploy:deploy-file -Dpackaging=pom -Dfile=pom.xml -Durl=https://artifactory.corp.adobe.com/artifactory/maven-aem-3rdparty-local -DrepositoryId=artifactory-dev-releases -DgroupId=com.adobe.aem.guides -DartifactId=aem-guides-wknd-shared -Dversion=$PACKAGE_VERSION
```

### Deploy content package

Navigate to `ui.content` folder

Copy the `pom.xml` file to target folder, making sure it gets renamed
```
cp pom.xml target/aem-guides-wknd-shared.ui.content-$PACKAGE_VERSION.pom
```

Deploy the package using

```
mvn -Partifactory-corp deploy:deploy-file -Dpackaging=zip -Dfile=target/aem-guides-wknd-shared.ui.content-$PACKAGE_VERSION.zip -DpomFile=target/aem-guides-wknd-shared.ui.content-$PACKAGE_VERSION.pom -Durl=https://artifactory.corp.adobe.com/artifactory/maven-aem-3rdparty-local -DrepositoryId=artifactory-dev-releases
```