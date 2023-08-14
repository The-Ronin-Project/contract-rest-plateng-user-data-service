# REST Contract for Platform-Engineering User Data Service

This repo contains the rest contract for `plateng-user-data-service`

# Tools

See [ronin-contract-rest-tooling](https://github.com/projectronin/ronin-contract-rest-tooling/blob/main/README.md) for more information about the tooling and how it works.

# Usage

## Project setup

The project looks like this:

```
├── .github                                    GitHub workflows
├── .gitignore                                 Generally applicable ignores
├── build.gradle.kts                           Gradle build script
├── settings.gradle.kts                        Gradle build settings script
└── v1
    ├── schemas
    │   └── sub-items.json                     Definitions for each of the sub-components of the API
    └── plateng-user-data-service.json         Overall definition of the user data service
```

When it comes time to make a new major version, you will need to create a new `v2` directory and put the new specification in there.


When you run the commands described below, these dependencies will be downloaded and placed in the `vN/.dependencies` directory based on the version you declared in your build file.  If they
are archives, they'll be unzipped, and you can reference the dependent contracts, schemas, etc. directly by path from inside your OpenAPI schema.

## Running

In general, either run:

`./gradlew <COMMAND> <ARGUMENTS>`

(requires JDK 11+ and node 16+ installed and working)

or

`./contract-tools <COMMAND> <ARGUMENTS>`

(requires: docker desktop or equivalent)

The two should be equivalent with the exception that the `contract-tools` script runs the gradle build inside a docker container.

Available commands are mostly general gradle commands.  Important or unusual ones are:

`incrementApiVersion [-Psnapshot=true|false] [-Pversion-increment=MINOR|PATCH|NONE]`: Increments the semantic versions of all API versions.  Optional
arguments determine if the tool puts a `-SNAPSHOT` on the end of thew version and what level of incrementing takes place.  Level `NONE` can be used to get
the tooling to remove an existing `-SNAPSHOT` suffix without making other changes.  The tool will _also_ make sure that the semantic version in each directory
matches the directory's major version number.

`check`: Verifies the contract using spectral tooling, making sure it's a valid contract.

`downloadApiDependencies`: downloads any API dependencies as specified in the `Dependencies` section above.

`build`: Runs `check`, generates schema documentation, and generates simple combined schema files.

`clean`: removes all generated files

`assemble`: Assembles the schema into deployable archives.

`publishToMavenLocal`: publishes all outputs to the local maven repo (e.g. `$HOME/.m2/repository`).  If you are using the docker image, it will try and
copy files in and out of the host's repository directory so they can be used for builds later on the host.

`publish`: publishes all outputs to the remote Ronin maven repository.
