# HiddenDoclet

This `javadoc` doclet is used to create an annotation which can exclude methods and variables from java documentation generation.

It is based on `ExcludeDoclet` that can be found at several places on the Internet, which use the `@exclude` tag.

However, since Java 9, the `@hidden` annotation was implemented and natively recognized by `javadoc`. Moreover, the new `javadoc` API 
used since Java 11 is not anymore compatible with the "old" `StandardDoclet`.

This `HiddenDoclet` allows using the `@hidden` tag in Java source code, independently of the JDK version that is used.

## For Java < 9
- Use the `@hidden` tag before any method or variable you don't want to appear in the API documentation of your application.
- Use the `HiddenDoclet` with `javadoc`.

A precompiled `HiddenDoclet.jar` is available in the "releases" section of this GitHub project. 

## For Java >= 9
- Use the `@hidden` tag before any method or variable you don't want to appear in the API documentation of your application.
- **Do not use** `HiddenDoclet`, as the `@hidden` tag is natively implemented.

## Example ANT task that automatically uses the right version

If no changes are needed in the java code of your classes, the parameters to use with `javadoc` depend on the JDK you use. 
It is, however, rather easy to automate the task, as in this example `ANT` snippet. 

A complete example can be found in the [Nodus GitHub project](https://github.com/jourquin/Nodus/blob/master/build-user.xml).


```
    <condition property="OldJavadocAPI">
        <contains string="${ant.java.version}" substring="1.8" />
    </condition>
    
    <target name="ApiDoc" description="Generates the javadoc API html documents."
        depends="ApiDoc-old, ApiDoc-new">
    </target>
          
    <target name="ApiDoc-old" description="Makes the javadoc API html documents." if="OldJavadocAPI">
        <javadoc sourcepath="${basedir}/src" destdir="${basedir}/api" maxmemory="256m" 
            encoding="UTF-8" docencoding="UTF-8" charset="UTF-8" 
            ...
            doclet="HiddenDoclet" docletpath="${basedir}/devtools/HiddenDoclet.jar"
            ...
    </target>
    
    <target name="ApiDoc-new" description="Makes the javadoc API html documents." unless="OldJavadocAPI">
        <javadoc sourcepath="${basedir}/src" destdir="${basedir}/api" maxmemory="256m" 
            encoding="UTF-8" docencoding="UTF-8" charset="UTF-8" 
            ...
    </target>
```

