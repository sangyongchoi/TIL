# enum-converter

코드상에서는 Enum 을 사용하지만 실제 저장될때는 임의의 값으로 변경하는 방법

1. Converter 를 만든다.

    ```kotlin
    @ReadingConverter
    class EnumReadingConverter : Converter<Int, Enum> {
        override fun convert(source: Int): Enum? =
            Enum.findByCode(source)
    }

    @WritingConverter
    class EnumWriterConverter : Converter<Enum, Int> {
        override fun convert(source: Enum): Int = source.code
    }
    ```

2. Mongo Config 에 해당 converter 를 추가한다.

    ```kotlin
    @Configuration
    class MongoConfig : AbstractMongoClientConfiguration() {

        override fun getDatabaseName(): String {
            return "databaseName"
        }

        override fun configureConverters(adapter: MongoConverterConfigurationAdapter) {
            adapter.registerConverter(EnumReadingConverter())
            adapter.registerConverter(EnumWriterConverter())
        }
    }
    ```