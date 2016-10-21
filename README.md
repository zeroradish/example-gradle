Codecov Java Example
====================

| [https://codecov.io][1] | [@codecov][2] | [hello@codecov.io][3] |
| ----------------------- | ------------- | --------------------- |

This repository serves as an **example** on how to use [Codecov Global][4] for Java with a [Gradle][5] build script.

## Usage


### Add Jacoco plugin
Gradle ships with a [JaCoCo plugin][6]. Just add it to your `build.gradle`.
```groovy
apply plugin: 'jacoco'
```

And enable the `jacoco` XML report.
```groovy
jacocoTestReport {
    reports {
        xml.enabled true
    }
}
```

# Circle CI

Add to your `circle.yml` file.
```yml
test:
    post:
        - if [ -e ./gradlew ]; then ./gradlew jacocoTestReport;else gradle jacocoTestReport;fi
        - bash <(curl -s https://codecov.io/bash)
```

> Another option is our [Bash uploader][7]

## Private Repos

Add to your `.travis.yml` file.
```yml
test:
    post:
        - if [ -e ./gradlew ]; then ./gradlew jacocoTestReport;else gradle jacocoTestReport;fi
        - bash <(curl -s https://codecov.io/bash) -t uuid-repo-token
```

View source and learn more about [Codecov Global Uploader][4]

## Multi-module projects (exmple with Travis CI)
Update your parent (root) `build.gradle`:
```groovy
allprojects {
    apply plugin: 'java'
    apply plugin: 'maven'
    apply plugin: 'jacoco'

    sourceCompatibility = 1.8
    targetCompatibility = 1.8

    repositories {
        mavenLocal()
        mavenCentral()
        jcenter()

        maven { url "http://repo1.maven.org/maven2/" }
    }
}

subprojects {
    dependencies {
        ...        
    }

    test.useTestNG()
}

task codeCoverageReport(type: JacocoReport) {
    executionData fileTree(project.rootDir.absolutePath).include("**/build/jacoco/*.exec")

    subprojects.each {
        sourceSets it.sourceSets.main
    }

    reports {
        xml.enabled true
        xml.destination "${buildDir}/reports/jacoco/report.xml"
        html.enabled false
        csv.enabled false
    }
}

codeCoverageReport.dependsOn {
    subprojects*.test
}
```

Update your `.travis.yml` file:
```yml
language: java
jdk:
  - oraclejdk8
before_script:
  - chmod +x gradlew
script:
  - ./gradlew check
  - ./gradlew codeCoverageReport
after_success:
  - bash <(curl -s https://codecov.io/bash)
```

No need to have anything else report-related in child modules 

[Credits for multi-module][8]


[1]: https://codecov.io/
[2]: https://twitter.com/codecov
[3]: mailto:hello@codecov.io
[4]: https://github.com/codecov/codecov-python
[5]: http://gradle.org/
[6]: https://docs.gradle.org/current/userguide/jacoco_plugin.html
[7]: https://github.com/codecov/codecov-bash
[8]: https://csiebler.github.io/blog/2014/02/09/multi-project-code-coverage-using-gradle-and-jacoco/