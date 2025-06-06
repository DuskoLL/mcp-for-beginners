<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "4d3415b9d2bf58bc69be07f945a69e07",
  "translation_date": "2025-05-20T23:44:15+00:00",
  "source_file": "09-CaseStudy/README.md",
  "language_code": "bg"
}
-->
# Казус: Azure AI Travel Agents – Референтна имплементация

## Преглед

[Azure AI Travel Agents](https://github.com/Azure-Samples/azure-ai-travel-agents) е цялостно референтно решение, разработено от Microsoft, което показва как да се създаде многоагентско приложение за планиране на пътувания с изкуствен интелект, използвайки Model Context Protocol (MCP), Azure OpenAI и Azure AI Search. Този проект демонстрира добри практики за координиране на множество AI агенти, интегриране на корпоративни данни и осигуряване на сигурна и разширяема платформа за реални сценарии.

## Основни характеристики
- **Оркестрация на множество агенти:** Използва MCP за координиране на специализирани агенти (напр. за полети, хотели и маршрути), които работят заедно при изпълнение на сложни задачи по планиране на пътуване.
- **Интеграция на корпоративни данни:** Свързва се с Azure AI Search и други корпоративни източници на данни, за да предоставя актуална и релевантна информация за препоръки при пътувания.
- **Сигурна и мащабируема архитектура:** Използва Azure услуги за удостоверяване, авторизация и мащабируемо внедряване, следвайки най-добрите практики за корпоративна сигурност.
- **Разширяеми инструменти:** Имплементира многократно използваеми MCP инструменти и шаблони за подсказки, позволяващи бързо адаптиране към нови области или бизнес изисквания.
- **Потребителско изживяване:** Предлага разговорен интерфейс, чрез който потребителите взаимодействат с агентите за пътувания, захранван от Azure OpenAI и MCP.

## Архитектура
![Architecture](https://raw.githubusercontent.com/Azure-Samples/azure-ai-travel-agents/main/docs/ai-travel-agents-architecture-diagram.png)

### Описание на архитектурната схема

Решението Azure AI Travel Agents е проектирано с модулност, мащабируемост и сигурна интеграция на множество AI агенти и корпоративни източници на данни. Основните компоненти и потокът на данни са както следва:

- **Потребителски интерфейс:** Потребителите взаимодействат със системата чрез разговорен интерфейс (като уеб чат или Teams бот), който изпраща заявки и получава препоръки за пътувания.
- **MCP сървър:** Служи като централен координиращ елемент, който приема вход от потребителя, управлява контекста и координира действията на специализираните агенти (например FlightAgent, HotelAgent, ItineraryAgent) чрез Model Context Protocol.
- **AI агенти:** Всеки агент отговаря за конкретна област (полети, хотели, маршрути) и е реализиран като MCP инструмент. Агентите използват шаблони за подсказки и логика за обработка на заявки и генериране на отговори.
- **Azure OpenAI Service:** Осигурява усъвършенствано разбиране и генериране на естествен език, позволявайки на агентите да интерпретират намеренията на потребителя и да отговарят разговорно.
- **Azure AI Search и корпоративни данни:** Агентите търсят в Azure AI Search и други корпоративни източници за актуална информация относно полети, хотели и опции за пътуване.
- **Удостоверяване и сигурност:** Интегрира се с Microsoft Entra ID за сигурно удостоверяване и прилага принципа на най-малко привилегии за достъп до всички ресурси.
- **Внедряване:** Проектирано за внедряване в Azure Container Apps, което осигурява мащабируемост, мониторинг и оперативна ефективност.

Тази архитектура позволява безпроблемна оркестрация на множество AI агенти, сигурна интеграция с корпоративни данни и стабилна, разширяема платформа за създаване на AI решения, специфични за конкретни области.

## Обяснение стъпка по стъпка на архитектурната схема
Представете си, че планирате голямо пътуване и имате екип от експертни помощници, които ви помагат с всеки детайл. Системата Azure AI Travel Agents работи по подобен начин, използвайки различни части (като членове на екипа), които имат специална роля. Ето как всичко се свързва:

### Потребителски интерфейс (UI):
Помислете за това като рецепцията на вашия туристически агент. Там вие (потребителят) задавате въпроси или правите заявки, като „Намери ми полет до Париж.“ Това може да е чат прозорец на уебсайт или приложение за съобщения.

### MCP сървър (Координаторът):
MCP сървърът е като мениджъра, който слуша вашата заявка на рецепцията и решава кой специалист да поеме всяка част. Той следи разговора ви и гарантира, че всичко протича гладко.

### AI агенти (Специализирани помощници):
Всеки агент е експерт в конкретна област — един знае всичко за полетите, друг за хотелите, а трети за планирането на маршрути. Когато поискате пътуване, MCP сървърът изпраща вашата заявка на съответния агент(и). Тези агенти използват знанията и инструментите си, за да намерят най-добрите опции за вас.

### Azure OpenAI Service (Експерт по езика):
Това е като да имате езиков експерт, който разбира точно какво питате, независимо как го формулирате. Той помага на агентите да разбират вашите заявки и да отговарят на естествен, разговорен език.

### Azure AI Search и корпоративни данни (Информационна библиотека):
Представете си огромна, актуална библиотека с най-новата информация за пътувания — разписания на полети, наличности на хотели и др. Агентите търсят в тази библиотека, за да ви дадат най-точните отговори.

### Удостоверяване и сигурност (Пазач):
Точно както пазач проверява кой може да влезе в определени зони, тази част гарантира, че само упълномощени лица и агенти имат достъп до чувствителна информация. Тя пази вашите данни сигурни и поверителни.

### Внедряване в Azure Container Apps (Сградата):
Всички тези помощници и инструменти работят заедно в сигурна, мащабируема „сграда“ (облак). Това означава, че системата може да обслужва много потребители едновременно и винаги е на разположение, когато имате нужда.

## Как всичко работи заедно:

Започвате, като зададете въпрос на рецепцията (UI).
Мениджърът (MCP сървър) определя кой специалист (агент) да ви помогне.
Специалистът използва езиковия експерт (OpenAI), за да разбере заявката ви, и библиотеката (AI Search), за да намери най-добрия отговор.
Пазачът (Удостоверяване) гарантира, че всичко е безопасно.
Всичко това се случва в надеждна, мащабируема „сграда“ (Azure Container Apps), за да бъде вашето изживяване гладко и сигурно.
Тази работа в екип позволява системата бързо и безопасно да ви помогне да планирате пътуването си, точно както екип от експертни туристически агенти, работещи заедно в модерен офис!

## Техническа имплементация
- **MCP сървър:** Хоства основната логика за оркестрация, предоставя агентски инструменти и управлява контекста за многостъпкови работни потоци за планиране на пътувания.
- **Агенти:** Всеки агент (например FlightAgent, HotelAgent) е реализиран като MCP инструмент със собствени шаблони за подсказки и логика.
- **Интеграция с Azure:** Използва Azure OpenAI за разбиране на естествен език и Azure AI Search за извличане на данни.
- **Сигурност:** Интегрира се с Microsoft Entra ID за удостоверяване и прилага контрол на достъпа с най-малко привилегии до всички ресурси.
- **Внедряване:** Поддържа внедряване в Azure Container Apps за мащабируемост и оперативна ефективност.

## Резултати и въздействие
- Демонстрира как MCP може да се използва за оркестрация на множество AI агенти в реален, продукционен сценарий.
- Ускорява разработката на решения чрез предоставяне на многократно използваеми модели за координация на агенти, интеграция на данни и сигурно внедряване.
- Служи като модел за изграждане на AI приложения, специфични за дадена област, използвайки MCP и Azure услуги.

## Референции
- [Azure AI Travel Agents GitHub хранилище](https://github.com/Azure-Samples/azure-ai-travel-agents)
- [Azure OpenAI Service](https://azure.microsoft.com/en-us/products/ai-services/openai-service/)
- [Azure AI Search](https://azure.microsoft.com/en-us/products/ai-services/ai-search/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

**Отказ от отговорност**:  
Този документ е преведен с помощта на AI преводаческа услуга [Co-op Translator](https://github.com/Azure/co-op-translator). Въпреки че се стремим към точност, моля, имайте предвид, че автоматизираните преводи могат да съдържат грешки или неточности. Оригиналният документ на неговия език трябва да се счита за авторитетен източник. За критична информация се препоръчва професионален човешки превод. Ние не носим отговорност за каквито и да е недоразумения или неправилни тълкувания, произтичащи от използването на този превод.