---
layout  : wiki
title   : 개발 프로세스 관련
summary : 안정적으로 확실하게 빠르게 개발하기
date    : 2018-05-18 10:17:13 +0900
updated : 2018-05-28 16:18:05 +0900
tags    : dev_flow
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 개요
개발은 더이상 혼자하지 않는다.
같이 개발하기 위해 서로 간의 약속을 정하고 지켜가야 사소한 시간을 아끼고
더욱 개발 고수가 되기 위한 시간을 보유할 수 있다고 생각한다.

그래서 개발을 시작해서 배포하기까지의 프로세스를 정립해보기로 했다.

## Elements
* DVCS -> Github
* Branch model -> Gif Flow
* Coding standard
    * coding style -> check style
    * static analysis -> PMD
    * Unit test -> Junit(java)
    * code coverage -> Jacoco(visualize -> codecov)
    * code review -> github pull request
    * git commit message -> ?(commitlint)
* CI -> travis CI

## flow
개발 프로세스는 
위의 요소들을 어떻게, 어디서, 언제 사용하나에 따라 신뢰성이 달라질적으로 판단된다.
local에서 개발할 경우의 process와 
local 작업 종료 후 remote에 push 할 경우 일어나는 프로세스로 구분

### LOCAL
1. coding style 을 유지하며 개발
2. 개발 후 static analysis(pmd) 를 통한 수정
3. unit test 를 실행
4. remote 로 push 전 commit message 확인
5. 개발자가 'git commit' or 'git push' 할때 위의 내용들을 git hock에서 재처리 함, 성공여부에 따라 실제 명렁어가 동작



### REMOTE (before 'git commit' or 'git push')
1. coding style 확인, 이상시 fail
2. static analysis(pmd) 확인, 이상시 fail
3. code coverage 확인, 기준을 못넘을 경우 fail
4. unit test 확인, 이상시 fail
5. Pull request 를 통한 code review
6. 이상없을 경우 끝!!!


## research

### check style
pom.xml 에 추가
```xml

<properties>
    <checkstyle.config.location>/src/main/resources/VendysStyleJavaCheckStyleConfig.xml</checkstyle.config.location>
    <maven.sevntu-checkstyle-check.checkstyle.version>8.9</maven.sevntu-checkstyle-check.checkstyle.version>
</properties>

<!-- Checkstyle -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>3.0.0</version>
    <dependencies>
        <dependency>
            <groupId>com.puppycrawl.tools</groupId>
            <artifactId>checkstyle</artifactId>
            <version>${maven.sevntu-checkstyle-check.checkstyle.version}</version>
        </dependency>
    </dependencies>
    <executions>
        <execution>
            <id>validate</id>
            <phase>validate</phase>
            <configuration>
                <encoding>UTF-8</encoding>
                <consoleOutput>true</consoleOutput>
                <failsOnError>true</failsOnError>
            </configuration>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>

```
maven cli
```
mvn checkstyle:checkstyle
mvn -e clean verify -DskipTests -DskipITs -Dpmd.skip=true -Dspotbugs.skip=true -Djacoco.skip=true"
```

### jacoco 
<!-- JaCoCo configuration -->
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>${maven.jacoco.plugin.version}</version>
    <executions>
        <execution>
            <id>prepare-agent</id>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>prepare-package</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
        <execution>
            <id>post-unit-test</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
            <configuration>
                <!-- Sets the path to the file which contains the execution data. -->

                <dataFile>target/jacoco.exec</dataFile>
                <!-- Sets the output directory for the code coverage report. -->
                <outputDirectory>target/jacoco-ut</outputDirectory>
            </configuration>
        </execution>
    </executions>
</plugin>


clean test -Plocal pmd:pm



// husky

npm install husky --save-dev


//commit lint
npm install --save-dev @commitlint/{config-conventional,cli}

echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js








































k

