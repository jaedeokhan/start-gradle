# start-gradle
[Gradle Docs](https://docs.gradle.org/7.5/userguide/userguide.html)를 보면서 공부한 내용을 정리한다.
Gradle Docs는 7.5버전을 참고해서 공부를 진행했다.

## gradle init
gradle init 명령어를 사용하면 gradle 초기 샘플 프로젝트를 간단하게 만들 수 있다.

```bash
gradle init
```

## java-app-sample [Building Java Applicatoins Sample](https://docs.gradle.org/7.5/samples/sample_building_java_applications.html)
gradle init 명령어를 통해서 java application sample 프로젝트를 따라해봤다
application을 생성하면 gradle 관련 폴더 및 쉘, app 밑에 소스, app/build.gradle, settings.gradle이 생성된다.

### settings.gradle

settings.gradle에는 rootProject.name과, include로 어떤 서브프로젝트를 구성하고 있는지 정의한다.
include가 한 개이면 싱글 프로젝트이고, 두 개 이상이면 멀티 프로젝트이다.
현재는 싱글 프로젝트로 진행했다.

```bash
rootProject.name = 'demo'
include('app')
```

### build.gradle
1. plugins에는 application을 정의했고, plugins은 다른 사람들이 이미 만들어 놓은 gradle task나 객체의 집합체로 유용한 기능들이 미리 정의되어져 있다.
2. repositories의 mavenCentral()는 https://repo.maven.apache.org/maven2/ 해당 경로로 기본 설정되어있고 정보들을 가져온다.
3. dependencies에는 의존성들을 정의한다.
4. application은 plugins인 application 기능이며 mainClass를 설정가능하다.
5. tasks.named('test')는 gradle task로 test를 만들고 useJunitPlatform()을 사용해서 test task 호출 시 Junit Platform을 이용한 테스트 수행을 가능하게 한다.

```bash
plugins {
    id 'application' 
}

repositories {
    mavenCentral() 
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter:5.8.2' 

    implementation 'com.google.guava:guava:31.0.1-jre' 
}

application {
    mainClass = 'demo.App' 
}

tasks.named('test') {
    useJUnitPlatform() 
}
```

### Run the application
gradle run task로 application을 실행한다.
실행한 결과가 나오게 된다.

```bash
gradle run
```

### Bundle the application
gradle build task를 실행하면 app/build/libs 밑에 jar가 생기고, app/build/distributions/app.tar or app.zip이 생긴다.

```bash
gradle build
```



