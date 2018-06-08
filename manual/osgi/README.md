# OSGi

The driver is available as an [OSGi] bundle.  More specifically, the following maven artifacts are valid OSGi bundles:

- `java-driver-core`
- `java-driver-query-builder`
- `java-driver-core-shaded`

## Using the shaded jar

`java-driver-core-shaded` shares the same bundle name as `java-driver-core` (`com.datastax.oss.driver.core`).  It can
be used as a drop-in replacement in cases where you have an explicit version of dependency in your project different
than that of the driver's.  Refer to [shaded jar](../core/shaded_jar/) for more information.

## What does the "Error loading libc" DEBUG message mean?

The driver is able to perform native system calls through [JNR] in some cases, for example to achieve microsecond
resolution when [generating timestamps](../core/query_timestamps/).

Unfortunately, some of the JNR artifacts available from Maven are not valid OSGi bundles and cannot be used in
OSGi applications.

[JAVA-1127] has been created to track this issue, and there is currently no simple workaround short of embedding
the dependency, which we've chosen not to do.

Because native calls are not available, it is also normal to see the following log lines when starting the driver:

    [main] DEBUG - Error loading libc
    java.lang.NoClassDefFoundError: jnr/ffi/LibraryLoader
    ...
    [main] INFO - Could not access native clock (see debug logs for details), falling back to Java system clock


[OSGi]:https://www.osgi.org
[JNR]: https://github.com/jnr/jnr-ffi
[JAVA-1127]:https://datastax-oss.atlassian.net/browse/JAVA-1127