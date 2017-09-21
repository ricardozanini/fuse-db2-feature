# DB2 Driver Feature

An Apache Karaf feature to add the OSGIfied version of DB2 driver to the container. This project does **not** provide the DB2 drivers, you should download it from the IBM website.

## How to use

Just run `mvn clean install` with the required properties (use the `-D` flag) and the `features.xml` file should be generated on your maven repository. Take the features file and add to the container profile:

1. `profile-edit --repository file:/path_to_feature/features.xml your-profile` (or the `mvn` path if you've installed the feature directly into the container repo);
2. `profile-edit --feature db2-jdbc your-profile [version]`

After adding the feature to your profile, the `osgi:list | grep -i db2` should look like this:

```shell
[ 169] [Active     ] [            ] [   80] db2jcc_license_cisuz (1.3.1)
[ 170] [Active     ] [            ] [   80] db2jcc4 (10.5)
```

There's six properties you have to define before generating the `feature.xml` file. Take a look at the table bellow:

| Property                      | Description | Example  |
|-------------------------------|-------------|-----------
|`db2.driver.locationURL`       |The URL where the DB2 driver is located. Should be a path where your container will look for the driver| `file:/${user.home}/drivers/db2jcc4-10.5.fp6.jar`|
|`db2.license.locationURL`      |The URL where the DB2 driver license is located.| `file:/${user.home}/drivers/db2jcc_license_cisuz-1.3.1.jar`|
|`db2.driver.bnd.locationURL`   |The URL where the DB2 driver bnd property file is located|`file:/${user.home}/drivers/db2jcc.bnd`|
|`db2.license.bnd.locationURL`  |The URL where the DB2 driver license bnd property file is located|`file:/${user.home}/drivers/db2jcc.bnd`|
|`db2.driver.version`           |The DB2 driver version. Take an extra care to not put versions there's not on the [semver](http://semver.org/) format.|
|`db2.license.version`          |The DB2 driver license version. Take an extra care to not put versions there's not on the [semver](http://semver.org/) format.|

This project already prepared the Bnd property file for this feature, so the container's wrap mechanism should work as is. There's a little catch on the import packages from the DB2 driver that shouldn't be available on the container nor on the driver itself. Just take the Bnd files from the `target/classes` directory after running the `mvn` command and add it to a place accessible to the server container.

## Credits

* [PAX-JDBC project](https://ops4j1.jira.com/wiki/spaces/PAXJDBC/pages/104103940/DB2+driver+adapter)
* [Bnd tool](http://bnd.bndtools.org/)