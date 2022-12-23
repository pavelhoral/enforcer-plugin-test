# Enforcer Plugin Check Issue

This repository showcases incorrect rule validation for `requirePluginVersions`.

Note that this is not a multimodule project, but a single repo with two projects:

* *parent* with parent POM
* *project* emulating parent POM for multimodule project

How to test:

```console
$ mvn install -f parent/pom.xml
$ mvn verify -f project/pom.xml
```

Expected result is for `mvn verify` to be successful. This is the case with Maven 3.x:

```console
$ mvn verify -f project/pom.xml
[INFO] Scanning for projects...
[INFO] 
[INFO] -----------------< org.example:enforcer-test-project >------------------
[INFO] Building enforcer-test-project 1.0.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] --- maven-enforcer-plugin:3.1.0:enforce (enforce-plugin-versions) @ enforcer-test-project ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.424 s
[INFO] Finished at: 2022-12-23T13:50:31+01:00
[INFO] ------------------------------------------------------------------------
```

However with Maven 4.0.0-alpha-3 we get:

```console
$ ../apache-maven-4.0.0-alpha-3/bin/mvn verify -f project/pom.xml
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------< org.example:enforcer-test-project >-------------------------------------------
[INFO] Building enforcer-test-project 1.0.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] ---------------------------------------------------------[ pom ]----------------------------------------------------------
[INFO] 
[INFO] --- enforcer:3.1.0:enforce (enforce-plugin-versions) @ enforcer-test-project ---
[ERROR] Rule 0: org.apache.maven.plugins.enforcer.RequirePluginVersions failed with message:
Some plugins are missing valid versions or depend on Maven 4.0.0-alpha-3 defaults: (LATEST RELEASE SNAPSHOT are not allowed)
org.apache.maven.plugins:maven-clean-plugin.    The version currently in use is 3.2.0 via super POM or default lifecycle bindings
org.apache.maven.plugins:maven-wrapper-plugin.  The version currently in use is 3.1.1 via super POM or default lifecycle bindings
org.apache.maven.plugins:maven-install-plugin.  The version currently in use is 3.0.1 via super POM or default lifecycle bindings
org.apache.maven.plugins:maven-site-plugin.     The version currently in use is 4.0.0-M4 via super POM or default lifecycle bindings
org.apache.maven.plugins:maven-deploy-plugin.   The version currently in use is 3.0.0 via super POM or default lifecycle bindings
org.apache.maven.plugins:maven-enforcer-plugin.         The version currently in use is 3.1.0 via super POM or default lifecycle bindings

[INFO] --------------------------------------------------------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] --------------------------------------------------------------------------------------------------------------------------
[INFO] Total time:  0.579 s
[INFO] Finished at: 2022-12-23T13:48:34+01:00
[INFO] --------------------------------------------------------------------------------------------------------------------------
```


