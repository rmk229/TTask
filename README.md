CrptApi

CrptApi - это Java-класс для работы с API Честного знака. Класс является потокобезопасным и поддерживает ограничение на количество запросов к API в определенный интервал времени.

 Оглавление

- [Установка](#установка)
- [Использование](#использование)
- [Конфигурация](#конфигурация)
- [Описание API](#описание-api)
- [Тестирование](#тестирование)
- [Авторы](#авторы)

## Установка

1. Убедитесь, что у вас установлена Java 17 или выше.
2. Склонируйте репозиторий проекта:
    ```sh
    git clone https://github.com/yourusername/crpt-api.git
    ```
3. Перейдите в директорию проекта:
    ```sh
    cd crpt-api
    ```
4. Соберите проект с помощью Maven:
    ```sh
    mvn clean install
    ```

Использование

Для использования CrptApi в вашем проекте, создайте объект класса `CrptApi` и вызовите метод `createDocument`.

Пример кода:

import kz.yermek.reestr.CrptApi;
import kz.yermek.reestr.CrptApi.Document;
import kz.yermek.reestr.CrptApi.Document.Product;

import java.util.Arrays;
import java.util.concurrent.TimeUnit;

public class Main {
    public static void main(String[] args) {
        CrptApi crptApi = new CrptApi(TimeUnit.MINUTES, 10); // Ограничение: 10 запросов в минуту

        Document document = new Document();
        document.setParticipantInn("1234567890");
        document.setDocId("doc123");
        document.setDocStatus("NEW");
        document.setDocType("LP_INTRODUCE_GOODS");
        document.setImportRequest(true);
        document.setOwnerInn("0987654321");
        document.setProducerInn("1122334455");
        document.setProductionDate("2022-01-01");
        document.setProductionType("TYPE");
        document.setProducts(Arrays.asList(new Product("doc", "2022-01-01", "123", "0987654321", "1122334455", "2022-01-01", "code", "uit", "uitu")));
        document.setRegDate("2022-01-01");
        document.setRegNumber("reg123");

        try {
            crptApi.createDocument(document, "signature123");
            System.out.println("Document created successfully.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
Конфигурация
CrptApi
Конструктор класса CrptApi:

java
Копировать код
public CrptApi(TimeUnit timeUnit, int requestLimit)
timeUnit - Единица времени (секунда, минута, час и т.д.).
requestLimit - Максимальное количество запросов в указанный интервал времени.
Документ
Класс Document представляет собой структуру данных для создания документа:

participantInn - ИНН участника.
docId - Идентификатор документа.
docStatus - Статус документа.
docType - Тип документа (по умолчанию "LP_INTRODUCE_GOODS").
importRequest - Флаг импорта.
ownerInn - ИНН владельца.
producerInn - ИНН производителя.
productionDate - Дата производства.
productionType - Тип производства.
products - Список продуктов (вложенный класс Product).
regDate - Дата регистрации.
regNumber - Номер регистрации.

Класс Product представляет собой структуру данных для продуктов:

certificateDocument - Документ сертификата.
certificateDocumentDate - Дата документа сертификата.
certificateDocumentNumber - Номер документа сертификата.
ownerInn - ИНН владельца.
producerInn - ИНН производителя.
productionDate - Дата производства.
tnvedCode - Код ТН ВЭД.
uitCode - Код УИТ.
uituCode - Код УИТУ.

Методы
createDocument
Метод для создания документа:
public void createDocument(Document document, String signature) throws IOException, InterruptedException
document - Объект класса Document.
signature - Подпись в виде строки.

Тестирование
Для тестирования кода используйте JUnit. Пример теста для метода createDocument:
import kz.yermek.reestr.CrptApi;
import kz.yermek.reestr.CrptApi.Document;
import kz.yermek.reestr.CrptApi.Document.Product;
import org.junit.jupiter.api.Test;

import java.util.Collections;
import java.util.concurrent.TimeUnit;

import static org.junit.jupiter.api.Assertions.assertDoesNotThrow;

public class CrptApiTest {

    @Test
    public void testCreateDocument() {
        CrptApi crptApi = new CrptApi(TimeUnit.MINUTES, 10);

        Document document = new Document();
        document.setParticipantInn("1234567890");
        document.setDocId("doc123");
        document.setDocStatus("NEW");
        document.setDocType("LP_INTRODUCE_GOODS");
        document.setImportRequest(true);
        document.setOwnerInn("0987654321");
        document.setProducerInn("1122334455");
        document.setProductionDate("2022-01-01");
        document.setProductionType("TYPE");
        document.setProducts(Collections.singletonList(new Product("doc", "2022-01-01", "123", "0987654321", "1122334455", "2022-01-01", "code", "uit", "uitu")));
        document.setRegDate("2022-01-01");
        document.setRegNumber("reg123");

        assertDoesNotThrow(() -> crptApi.createDocument(document, "signature123"));
    }
}

Авторы
Ermek Shmanov
Лицензия
Этот проект лицензирован в соответствии с условиями лицензии MIT. Подробности см. в файле LICENSE.

Контакты
Если у вас есть вопросы или предложения по улучшению проекта, пожалуйста, свяжитесь со мной по адресу shmanovermek@gmail.com
