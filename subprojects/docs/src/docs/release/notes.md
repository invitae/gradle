The Gradle team is excited to announce Gradle @version@.

This release features [1](), [2](), ... [n](), and more.

We would like to thank the following community contributors to this release of Gradle:
<!-- 
Include only their name, impactful features should be called out separately below.
 [Some person](https://github.com/some-person)
-->
[Andrew K.](https://github.com/miokowpak)
[Dan Sănduleac](https://github.com/dansanduleac),
[Martin d'Anjou](https://github.com/martinda),
[Ben Asher](https://github.com/benasher44),
[Mike Kobit](https://github.com/mkobit),
[Erhard Pointl](https://github.com/epeee),
[Sebastian Schuberth](https://github.com/sschuberth),
[Evgeny Mandrikov](https://github.com/Godin),
[Stefan M.](https://github.com/StefMa),
[Igor Melnichenko](https://github.com/igor-melnichenko),
[Björn Kautler](https://github.com/Vampire),
[Roberto Perez Alcolea](https://github.com/rpalcolea) and
[Christian Fränkel](https://github.com/fraenkelc).

details of 2

## n
-->

## Upgrade Instructions

Switch your build to use Gradle @version@ by updating your wrapper:

`./gradlew wrapper --gradle-version=@version@`

See the [Gradle 5.x upgrade guide](userguide/upgrading_version_5.html#changes_@baseVersion@) to learn about deprecations, breaking changes and other considerations when upgrading to Gradle @version@.

**NOTE:** Gradle 5.5 has had _one_ patch release. We recommend using the latest patch release.

<!-- Do not add breaking changes or deprecations here! Add them to the upgrade guide instead. --> 

<a name="artifact-transforms"/>

## Transforming dependency artifacts on resolution

A dependency’s artifacts can take many forms, and sometimes you might need one that is not readily available.
As an example, imagine you have a dependency on a Java module.
Its producer publishes a normal JAR file and an obfuscated JAR file to a binary repository.
As a consumer, you want to use the normal JAR for development.
But for production, you want to use an obfuscated version of the JAR rather than the version that’s published.

Let’s say you want to get hold of the obfuscated JAR, but it’s not available in any repository.
Why not just retrieve the un-obfuscated JAR and obfuscate it yourself?

Gradle now allows you to register an _artifact transform_ to do just that, by hooking into the dependency management resolution engine.
You can specify that whenever an obfuscated JAR is requested but can’t be found, Gradle should run an artifact transform that performs the obfuscation and makes the resulting artifact available transparently to the build.
 
For more information have a look at the [user manual](userguide/dependency_management_attribute_based_matching.html#sec:abm_artifact_transforms).

<a name="build-init"/>

## Build init plugin improvements

### Support for JUnit Jupiter

The `init` task now provides an option to use Junit Jupiter, instead of Junit 4, to test Java applications and libraries. You can select this test framework when you run the `init` task interactively, or use the `--test-framework` command-line option. See the [User manual](userguide/build_init_plugin.html) for more details.

Contributed by [Erhard Pointl](https://github.com/epeee)

### Task dependencies are honored for `@Input` properties of type `Property`

TBD - honors dependencies on `@Input` properties.

### Property methods

<a name="gradle-properties"/>

## Define organization-wide properties with a custom Gradle Distribution

Gradle now looks for a `gradle.properties` file in the Gradle distribution used by the build.  This file has the [lowest precedence of any `gradle.properties`](userguide/build_environment.html#sec:gradle_configuration_properties) and properties defined in other locations will override values defined here.

By placing a `gradle.properties` file in a [custom Gradle distribution](userguide/organizing_gradle_projects.html#sec:custom_gradle_distribution), an organization can add default properties for the entire organization or tweak the default Gradle daemon memory parameters with `org.gradle.jvmargs`.

<a name="improvements-plugin-authors"/>

## Improvements for plugin authors

### Abstract service injection methods

Gradle provides several useful services that are available for injection into various custom types, including task types, plugins and project extensions. One such service, for example, is the  
`ObjectFactory` type.

In previous Gradle versions, a type could receive a service by declaring a property getter with a dummy method body. Now, these property getters can be abstract, which is more convenient and clearer.

See the [user manual](userguide/custom_gradle_types.html#service_injection) for more details and examples.

### `ObjectFactory` methods for creating domain object collections

Gradle provides a number of types that plugin authors can use to manage a collection of objects. These are called the "domain object collection" types and provide useful features such as DSL support 
and lazy configuration. These types are used extensively throughout the Gradle API, for example `project.tasks` and `project.repositories` are domain object collections.  

Previously, it was only possible to create a domain object collection by using the APIs provided by a `Project`. However, a `Project` object is not always available, for example
in a `Settings` plugin or in a project extension object.

The [`ObjectFactory`](javadoc/org/gradle/api/model/ObjectFactory.html) service now has methods for creating [`NamedDomainObjectContainer`](javadoc/org/gradle/api/NamedDomainObjectContainer.html) and [`DomainObjectSet`](javadoc/org/gradle/api/DomainObjectSet.html) instances.
This means that any code where a `ObjectFactory` is available can now create collection instances. 

See the [user manual](userguide/custom_gradle_types.html#collection_types) for more details and examples.

### Abstract model properties

A custom type, such as a task type, plugin or project extension can now be implemented as an abstract class or, in the case of project extensions and other data types, an interface.
Gradle can provide an implementation for abstract properties.

For a property with abstract getter and setter methods, Gradle will provide implementations for the getter and setters.

For a read only property with an abstract getter method, Gradle will provide an implementation for the getter method and also create a value for the property.
For plugin authors, this simplifies the process of providing configuration properties for a task or project extension.

See the [user manual](userguide/custom_gradle_types.html#managed_properties) for more details and examples.

### Additional documentation

There is a new user manual [chapter](userguide/custom_gradle_types.html) that describes how to use these features in your custom Gradle types.

<a name="native-support"/>

## Building native software with Gradle

All new C++ documentations including new user manual chapters for [building](userguide/building_cpp_projects.html) and [testing](userguide/cpp_testing.html) C++ projects, [DSL reference for C++ components](dsl/index.html#N10808), [C++ plugins reference chapters](userguide/plugin_reference.html#native_languages) and [Visual Studio and Xcode IDE plugins reference chapters](userguide/plugin_reference.html#ide_integration).
The [C++ guides](https://gradle.org/guides/?q=Native) were also improved to reflect all the new features available to C++ developers.
See more information about the [Gradle native project](https://github.com/gradle/gradle-native/blob/master/docs/RELEASE-NOTES.md#changes-included-in-gradle-55).

### Update to the Scala Zinc compiler

The Scala Zinc compiler has been upgraded to version 1.2.5, which only supports Scala 2.10+, meaning that Gradle no longer supports building for Scala 2.9.
This fixes some Scala incremental compilation bugs and is reported as significantly improving performance.
If you used to override the Zinc compiler version, be aware that the minimal version is now 1.2.0.

## Improved Eclipse project name deduplication in Buildship

When importing Gradle Eclipse projects into Buildship, the current Eclipse workpace state is taken into account. This allows Gradle to import/synchronize in Eclipse workspaces that include
non-Gradle projects that conflict with project names in the imported project.

The upcoming 3.1.1 version of Buildship is required to take advantage of this behavior.

Contributed by [Christian Fränkel](https://github.com/fraenkelc)

## Gradle Kotlin DSL compiler upgraded to Kotlin 1.3.31

The Gradle Kotlin DSL embedded Kotlin compiler has been upgraded from version `1.2.21` to version `1.3.31`, please refer to the [Kotlin 1.3.30 release blog entry](https://blog.jetbrains.com/kotlin/2019/04/kotlin-1-3-30-released/) and the [Kotlin 1.3.31 GitHub release notes](https://github.com/JetBrains/kotlin/releases/tag/v1.3.31) for details.

## Fixed issues

## Known issues

Known issues are problems that were discovered post release that are directly related to changes made in this release.

## External contributions

We love getting contributions from the Gradle community. For information on contributing, please see [gradle.org/contribute](https://gradle.org/contribute).

## Reporting Problems

If you find a problem with this release, please file a bug on [GitHub Issues](https://github.com/gradle/gradle/issues) adhering to our issue guidelines. 
If you're not sure you're encountering a bug, please use the [forum](https://discuss.gradle.org/c/help-discuss).

We hope you will build happiness with Gradle, and we look forward to your feedback via [Twitter](https://twitter.com/gradle) or on [GitHub](https://github.com/gradle).
