# 2021-05-12

Spring Rest Docs 란 ?

문서화를 손쉽게 할 수 있게 도와주는 서비스

출처 :  [https://spring.io/projects/spring-restdocs](https://spring.io/projects/spring-restdocs)

설정 방법 (with kotlin)

1. build.gradle.kts 설정

    ```jsx
    dependencies {
        ...
        testImplementation("org.springframework.restdocs:spring-restdocs-mockmvc")
    }

    ...
    tasks {
        asciidoctor {
            dependsOn(test)
            inputs.dir(ext.get("snippetsDir") as File)

            doFirst {
                delete(file("src/main/resources/static/docs"))
            }

            doLast {
                println("===== END asciidoctor GENERATE =======")
            }
        }

        register("copyDocument", Copy::class) {
            dependsOn(asciidoctor)
            from("build/asciidoc/html5")
            into("src/main/resources/static/docs")
        }

        build {
            dependsOn(":copyDocument")
        }

        bootJar {
            dependsOn(asciidoctor)
            from ("${asciidoctor.get().outputDir}/html5") {
                into("BOOT-INF/classes/static/docs")
            }
            archiveFileName.set("app.jar")
        }
    }
    ```

    출처 : [https://github.com/TASK-FORCE/mannalga-api](https://github.com/TASK-FORCE/mannalga-api)

2. 폴더 생성 및 파일 생성
보이는 것과 같이 폴더와 파일을 생성한다.

    ![2021-05-12%2099104da8990440c684e8e369a5c545d1/_2021-05-12__6.57.04.png](2021-05-12%2099104da8990440c684e8e369a5c545d1/_2021-05-12__6.57.04.png)

3. index.adoc 파일 작성
예제입니다.
snippets 의 경로를 따로 안잡아줘도 동작하는 분들도 계시지만 
저는 파일을 찾을 수 없다는 에러가 뜨기에 경로를 잡아줬습니다.

    ```jsx
    :snippets: ../../../build/generated-snippets
    = User

    == User 가입

    REQUEST:
    include::{snippets}/signup/http-request.adoc[]
    include::{snippets}/signup/request-fields.adoc[]

    RESPONSE:
    include::{snippets}/signup/http-response.adoc[]
    ```

4. 빌드
1. asciidoctor
2. build
를 차례대로 해줍니다.

    ![2021-05-12%2099104da8990440c684e8e369a5c545d1/_2021-05-12__7.04.22.png](2021-05-12%2099104da8990440c684e8e369a5c545d1/_2021-05-12__7.04.22.png)

5. 확인
도메인/docs/index.html 로 접속해서 확인합니다.