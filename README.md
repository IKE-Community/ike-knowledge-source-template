# TEMPLATE_PROJECT_NAME

Knowledge source artifact for TEMPLATE_PROJECT_NAME.

## Overview

This project packages knowledge source files (RF2, OWL, CSV, etc.) as a Maven artifact for use by downstream projects. Source files are **not** checked into git due to their size - they are downloaded locally, packaged, and deployed to the Maven repository.

## Usage

### Downloading as Dependency

Add to your project's POM:

```xml
<dependency>
    <groupId>TEMPLATE_GROUP_ID</groupId>
    <artifactId>TEMPLATE_ARTIFACT_ID</artifactId>
    <version>TEMPLATE_VERSION</version>
    <type>zip</type>
</dependency>
```

Use the `maven-dependency-plugin` to unpack:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <executions>
        <execution>
            <id>unpack-source</id>
            <phase>generate-resources</phase>
            <goals>
                <goal>unpack</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>TEMPLATE_GROUP_ID</groupId>
                        <artifactId>TEMPLATE_ARTIFACT_ID</artifactId>
                        <version>TEMPLATE_VERSION</version>
                        <type>zip</type>
                        <outputDirectory>${project.build.directory}/knowledge-source</outputDirectory>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
```

## Building and Deploying

### Prerequisites

- Java 25
- Maven 4 (via wrapper)
- Source files downloaded to `source/` directory

### Building

1. **Download source files** into the `source/` directory:
   ```bash
   # Example for SNOMED CT
   # Download RF2 release from SNOMED International
   # Unzip to source/SnomedCT_...
   ```

2. **Build the artifact**:
   ```bash
   ./mvnw clean package
   ```
   
   This creates `target/TEMPLATE_ARTIFACT_ID-TEMPLATE_VERSION.zip`

3. **Deploy to Maven repository**:
   ```bash
   ./mvnw clean deploy
   ```

### Releasing

Use GitFlow to create versioned releases:

```bash
# Start a release
./mvnw gitflow:release-start

# The release branch is created with the release version
# Verify the build works
./mvnw clean package

# Finish the release (merges to main, tags, deploys)
./mvnw gitflow:release-finish
```

## Project Structure

```
TEMPLATE_ARTIFACT_ID/
├── pom.xml                    # Maven configuration (packaging: zip)
├── src/
│   └── assembly/
│       └── source-zip.xml     # Assembly descriptor
├── source/                    # Knowledge source files (gitignored)
│   └── .gitkeep              # Placeholder to keep directory in git
├── .gitignore                 # Ignores source files
└── README.md
```

## Source File Guidelines

- Place source files directly in the `source/` directory
- All files in `source/` (except `.gitkeep`) are packaged into the zip
- The zip contains the source files at the root level (no wrapper directory)
- Do not commit source files to git - they should be downloaded fresh for each build

## License

Apache License, Version 2.0
