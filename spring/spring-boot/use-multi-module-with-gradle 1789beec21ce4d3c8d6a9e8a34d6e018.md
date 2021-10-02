# use-multi-module-with-gradle

IntelliJ 기준으로 설명하겠습니다.

1. 새로운 프로젝트를 만듭니다.
2. 만들어진 프로젝트 우클릭 → New → Module 로 원하는 프로젝트를 만듭니다.
필자는 user 라는 모듈을 만들었습니다.
3. settings.gradle.kts 파일의 내용을 아래와 같이 수정합니다.
    
    ```kotlin
    pluginManagement {
        val kotlinVersion: String by settings
        plugins {
            kotlin("jvm") version kotlinVersion
            kotlin("plugin.allopen") version kotlinVersion
            kotlin("plugin.noarg") version kotlinVersion
            kotlin("kapt") version kotlinVersion
    
            kotlin("plugin.jpa") version kotlinVersion
            kotlin("plugin.spring") version kotlinVersion
        }
    }
    
    rootProject.name = "saga"
    
    include("user")
    ```
    
4. [gradle.properties](http://gradle.properties) 파일을 생성하고 아래와 같이 작성해줍니다.
    
    ```kotlin
    kotlin.code.style=official
    version=0.0.1
    kotlinVersion=1.4.20
    ```
    
5. build.gradle.kts 파일을 아래와같이 수정하여줍니다.
    
    ```kotlin
    plugins{
    	base
    	id("org.springframework.boot")version"2.5.5"applyfalse
      id("io.spring.dependency-management")version"1.0.11.RELEASE"applyfalse
    	kotlin("jvm")version"1.5.31"applyfalse
    	kotlin("plugin.spring")version"1.5.31"applyfalse
    }
    
    group= "com.example"
    version= "0.0.1-SNAPSHOT"
    
    allprojects{
    	repositories{
    		mavenCentral()
    	}
    }
    ```
    
6. user/build.gradle.kts 를 아래와 같이 수정해줍니다.
    
    ```kotlin
    import org.jetbrains.kotlin.gradle.tasks.KotlinCompile
    
    plugins {
        id("org.springframework.boot")
        id("io.spring.dependency-management")
        kotlin("jvm")
        kotlin("plugin.spring")
        kotlin("plugin.jpa")
    }
    
    group = "com.example"
    version = "0.0.1-SNAPSHOT"
    
    repositories {
        mavenCentral()
    }
    
    dependencies {
        implementation("org.springframework.boot:spring-boot-starter-data-jpa")
        implementation("org.springframework.boot:spring-boot-starter-web")
        implementation("com.fasterxml.jackson.module:jackson-module-kotlin")
        implementation("org.jetbrains.kotlin:kotlin-reflect")
        implementation("org.jetbrains.kotlin:kotlin-stdlib-jdk8")
        implementation("org.springframework.kafka:spring-kafka")
        runtimeOnly("mysql:mysql-connector-java")
        testImplementation("org.springframework.boot:spring-boot-starter-test")
        testImplementation("org.springframework.kafka:spring-kafka-test")
    }
    
    tasks {
        withType<KotlinCompile> {
            kotlinOptions {
                freeCompilerArgs = listOf("-Xjsr305=strict")
                jvmTarget = "11"
            }
        }
    
        withType<Test> {
            useJUnitPlatform()
        }
    }
    ```