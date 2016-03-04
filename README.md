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
        - pip install --user codecov && codecov
```

> Another option is our [Bash uploader][7]

## Private Repos

Add to your `.travis.yml` file.
```yml
test:
    post:
        - if [ -e ./gradlew ]; then ./gradlew jacocoTestReport;else gradle jacocoTestReport;fi
        - pip install --user codecov && codecov:
            environment:
                CODECOV_TOKEN: uuid-repo-token
```

View source and learn more about [Codecov Global Uploader][4]

[1]: https://codecov.io/
[2]: https://twitter.com/codecov
[3]: mailto:hello@codecov.io
[4]: https://github.com/codecov/codecov-python
[5]: http://gradle.org/
[6]: https://docs.gradle.org/current/userguide/jacoco_plugin.html
[7]: https://github.com/codecov/codecov-bash
