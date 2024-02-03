# start-gradle
[Gradle Docs](https://docs.gradle.org/7.5/userguide/userguide.html)를 보면서 공부한 내용을 정리한다.
Gradle Docs는 7.5버전을 참고해서 공부를 진행했다.

## Build Phases
1. Phase 1. Initialization
   - settings.gradle 파일을 감지한다.
   - Settings 인스턴스를 생성한다.
   - 설정 파일을 평가하여 빌드를 구성하는 프로젝트 및 포함된 빌드를 결정한다.
   - 모든 프로젝트에 대한 Project 인스턴스를 생성한다.
2. Phase 2. Configuration
   - build.gradle에 있는 모든 프로젝트의 빌드 스크립트를 평가한다.
   - 요청된 작업에 대한 작업 그래프를 생성한다. (방향성 비순환 그래프 - DAG)
3. Phase 3. Execution
   - 선택한 task를 예약하고 실행한다.
   - task간의 종속성에 따라 실행 순서가 결정된다.
   - task 실행은 병렬로 발생할 수 있다.

![image](https://github.com/jaedeokhan/start-gradle/assets/45028904/a584a1b0-84dd-406b-b258-58e3ecc8e3c9)

## Gradle Build DAG & gradle build 순환도
Gradle의 task는 방향성 비순환 그래프를 그린다.
gradle build 태스크를 실행하면 하단 task들의 역순으로 동작한다.
1. compileJava, processResources
2. classes
3. jar
4. test
5. assemble
6. check
7. build

gradle build 명령어를 치고 상세 내용을 보고 싶으면 옵션으로 --info를 주면 task 실행 상세 정보를 확인 가능하다.
--info 옵션을 사용하면 엄청 길게 나온다.

```bash
./gradlew build --info

Initialized native services in: C:\Users\jaedeok\.gradle\native
Initialized jansi services in: C:\Users\jaedeok\.gradle\native
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Using 8 worker leases.
Now considering [C:\Users\jaedeok\gradle-sample\start-gradle\java-app-sample, C:\Users\jaedeok\gradle-sample\start-gradle\java-app-and-lib\buildSrc, C:\Users\jaedeok\gradle-sample\start-gradle\java-app-and-lib] as hierarchies to watch
Watching the file system is configured to be enabled if available
File system watching is active
Starting Build
Settings evaluated using settings file 'C:\Users\jaedeok\gradle-sample\start-gradle\java-app-sample\settings.gradle'.
Projects loaded. Root project using build file 'C:\Users\jaedeok\gradle-sample\start-gradle\java-app-sample\build.gradle'.
Included projects: [root project 'demo', project ':app']

> Configure project :
Evaluating root project 'demo' using build file 'C:\Users\jaedeok\gradle-sample\start-gradle\java-app-sample\build.gradle'.

> Configure project :app
Evaluating project ':app' using build file 'C:\Users\jaedeok\gradle-sample\start-gradle\java-app-sample\app\build.gradle'.
All projects evaluated.
Task name matched 'build'
Selected primary task 'build' from project :
Tasks to be executed: [task ':app:compileJava', task ':app:processResources', task ':app:classes', task ':app:jar', task ':app:startScripts', task ':app:distTar', task ':app:distZip', task ':app:assemble', task ':app:compileTestJava', task ':app:processTestResources', task ':app:testClasses', task ':app:test', task ':app:check', task ':app:build']
Tasks that were excluded: []
Resolve mutations for :app:compileJava (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:compileJava (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:compileJava (Thread[included builds,5,main]) started.

> Task :app:compileJava UP-TO-DATE
Caching disabled for task ':app:compileJava' because:
  Build cache is disabled
Skipping task ':app:compileJava' as it is up-to-date.
:app:compileJava (Thread[included builds,5,main]) completed. Took 0.016 secs.
Resolve mutations for :app:processResources (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:processResources (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:processResources (Thread[included builds,5,main]) started.

> Task :app:processResources NO-SOURCE
Skipping task ':app:processResources' as it has no source files and no previous output files.
:app:processResources (Thread[included builds,5,main]) completed. Took 0.0 secs.
Resolve mutations for :app:classes (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:classes (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:classes (Thread[included builds,5,main]) started.

> Task :app:classes UP-TO-DATE
Skipping task ':app:classes' as it has no actions.
:app:classes (Thread[included builds,5,main]) completed. Took 0.0 secs.
Resolve mutations for :app:jar (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:jar (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:jar (Thread[included builds,5,main]) started.

> Task :app:jar UP-TO-DATE
Caching disabled for task ':app:jar' because:
  Build cache is disabled
Skipping task ':app:jar' as it is up-to-date.
:app:jar (Thread[included builds,5,main]) completed. Took 0.003 secs.
Resolve mutations for :app:startScripts (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:startScripts (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:startScripts (Thread[included builds,5,main]) started.

> Task :app:startScripts UP-TO-DATE
Caching disabled for task ':app:startScripts' because:
  Build cache is disabled
Skipping task ':app:startScripts' as it is up-to-date.
:app:startScripts (Thread[included builds,5,main]) completed. Took 0.004 secs.
Resolve mutations for :app:distTar (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:distTar (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:distTar (Thread[included builds,5,main]) started.

> Task :app:distTar UP-TO-DATE
Caching disabled for task ':app:distTar' because:
  Build cache is disabled
Skipping task ':app:distTar' as it is up-to-date.
:app:distTar (Thread[included builds,5,main]) completed. Took 0.001 secs.
Resolve mutations for :app:distZip (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:distZip (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:distZip (Thread[included builds,5,main]) started.

> Task :app:distZip UP-TO-DATE
Caching disabled for task ':app:distZip' because:
  Build cache is disabled
Skipping task ':app:distZip' as it is up-to-date.
:app:distZip (Thread[included builds,5,main]) completed. Took 0.001 secs.
Resolve mutations for :app:assemble (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:assemble (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:assemble (Thread[included builds,5,main]) started.

> Task :app:assemble UP-TO-DATE
Skipping task ':app:assemble' as it has no actions.
:app:assemble (Thread[included builds,5,main]) completed. Took 0.0 secs.
Resolve mutations for :app:compileTestJava (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:compileTestJava (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:compileTestJava (Thread[included builds,5,main]) started.

> Task :app:compileTestJava UP-TO-DATE
Caching disabled for task ':app:compileTestJava' because:
  Build cache is disabled
Skipping task ':app:compileTestJava' as it is up-to-date.
:app:compileTestJava (Thread[included builds,5,main]) completed. Took 0.007 secs.
Resolve mutations for :app:processTestResources (Thread[Execution worker,5,main]) started.
Resolve mutations for :app:processTestResources (Thread[Execution worker,5,main]) completed. Took 0.0 secs.
:app:processTestResources (Thread[included builds,5,main]) started.

> Task :app:processTestResources NO-SOURCE
Skipping task ':app:processTestResources' as it has no source files and no previous output files.
:app:processTestResources (Thread[included builds,5,main]) completed. Took 0.0 secs.
Resolve mutations for :app:testClasses (Thread[Execution worker,5,main]) started.
:app:testClasses (Thread[included builds,5,main]) started.
Resolve mutations for :app:testClasses (Thread[Execution worker,5,main]) completed. Took 0.0 secs.

> Task :app:testClasses UP-TO-DATE
Skipping task ':app:testClasses' as it has no actions.
:app:testClasses (Thread[included builds,5,main]) completed. Took 0.0 secs.
Resolve mutations for :app:test (Thread[Execution worker Thread 3,5,main]) started.
Resolve mutations for :app:test (Thread[Execution worker Thread 3,5,main]) completed. Took 0.0 secs.
producer locations for task group 0 (Thread[Execution worker Thread 2,5,main]) started.
:app:test (Thread[included builds,5,main]) started.
producer locations for task group 0 (Thread[Execution worker Thread 2,5,main]) completed. Took 0.0 secs.

> Task :app:test UP-TO-DATE
Caching disabled for task ':app:test' because:
  Build cache is disabled
Skipping task ':app:test' as it is up-to-date.
:app:test (Thread[included builds,5,main]) completed. Took 0.004 secs.
Resolve mutations for :app:check (Thread[Execution worker Thread 2,5,main]) started.
Resolve mutations for :app:check (Thread[Execution worker Thread 2,5,main]) completed. Took 0.0 secs.
:app:check (Thread[included builds,5,main]) started.

> Task :app:check UP-TO-DATE
Skipping task ':app:check' as it has no actions.
:app:check (Thread[included builds,5,main]) completed. Took 0.0 secs.
Resolve mutations for :app:build (Thread[Execution worker Thread 2,5,main]) started.
Resolve mutations for :app:build (Thread[Execution worker Thread 2,5,main]) completed. Took 0.0 secs.
:app:build (Thread[included builds,5,main]) started.

> Task :app:build UP-TO-DATE
Skipping task ':app:build' as it has no actions.
:app:build (Thread[included builds,5,main]) completed. Took 0.0 secs.
```


![image](https://github.com/jaedeokhan/start-gradle/assets/45028904/34de312e-0586-4699-a1a9-a3ba4d64e450)



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



