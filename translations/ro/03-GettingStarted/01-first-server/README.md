<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "bf05718d019040cf0c7d4ccc6d6a1a88",
  "translation_date": "2025-06-13T06:08:06+00:00",
  "source_file": "03-GettingStarted/01-first-server/README.md",
  "language_code": "ro"
}
-->
### -2- Creează proiectul

Acum că ai instalat SDK-ul, să creăm următorul proiect:

### -3- Creează fișierele proiectului

### -4- Creează codul serverului

### -5- Adaugă un tool și o resursă

Adaugă un tool și o resursă inserând următorul cod:

### -6 Codul final

Să adăugăm ultimul cod necesar pentru ca serverul să pornească:

### -7- Testează serverul

Pornește serverul cu următoarea comandă:

### -8- Rulează folosind inspectorul

Inspectorul este un instrument excelent care poate porni serverul și îți permite să interacționezi cu el pentru a testa dacă funcționează. Să-l pornim:

> [!NOTE]
> Comanda din câmpul "command" poate arăta diferit, deoarece conține comanda specifică pentru runtime-ul tău.

Ar trebui să vezi următoarea interfață:

![Connect](../../../../translated_images/connect.141db0b2bd05f096fb1dd91273771fd8b2469d6507656c3b0c9df4b3c5473929.ro.png)

1. Conectează-te la server selectând butonul Connect  
   După ce te conectezi, ar trebui să vezi următorul ecran:

   ![Connected](../../../../translated_images/connected.73d1e042c24075d386cacdd4ee7cd748c16364c277d814e646ff2f7b5eefde85.ro.png)

2. Selectează "Tools" și apoi "listTools", ar trebui să apară opțiunea "Add", selecteaz-o și completează valorile parametrilor.

   Vei vedea următorul răspuns, adică rezultatul rulării tool-ului "add":

   ![Result of running add](../../../../translated_images/ran-tool.a5a6ee878c1369ec1e379b81053395252a441799dbf23416c36ddf288faf8249.ro.png)

Felicitări, ai reușit să creezi și să rulezi primul tău server!

### SDK-uri oficiale

MCP oferă SDK-uri oficiale pentru mai multe limbaje:
- [C# SDK](https://github.com/modelcontextprotocol/csharp-sdk) - Întreținut în colaborare cu Microsoft
- [Java SDK](https://github.com/modelcontextprotocol/java-sdk) - Întreținut în colaborare cu Spring AI
- [TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk) - Implementarea oficială TypeScript
- [Python SDK](https://github.com/modelcontextprotocol/python-sdk) - Implementarea oficială Python
- [Kotlin SDK](https://github.com/modelcontextprotocol/kotlin-sdk) - Implementarea oficială Kotlin
- [Swift SDK](https://github.com/modelcontextprotocol/swift-sdk) - Întreținut în colaborare cu Loopwork AI
- [Rust SDK](https://github.com/modelcontextprotocol/rust-sdk) - Implementarea oficială Rust

## Aspecte esențiale

- Configurarea unui mediu de dezvoltare MCP este simplă cu SDK-urile specifice fiecărui limbaj
- Construirea serverelor MCP implică crearea și înregistrarea tool-urilor cu scheme clare
- Testarea și depanarea sunt esențiale pentru implementări MCP fiabile

## Exemple

- [Calculator Java](../samples/java/calculator/README.md)
- [Calculator .Net](../../../../03-GettingStarted/samples/csharp)
- [Calculator JavaScript](../samples/javascript/README.md)
- [Calculator TypeScript](../samples/typescript/README.md)
- [Calculator Python](../../../../03-GettingStarted/samples/python)

## Tema

Creează un server MCP simplu cu un tool la alegere:
1. Implementează tool-ul în limbajul preferat (.NET, Java, Python sau JavaScript).
2. Definește parametrii de intrare și valorile de retur.
3. Rulează instrumentul inspector pentru a verifica funcționarea serverului.
4. Testează implementarea cu diferite intrări.

## Soluție

[Soluție](./solution/README.md)

## Resurse suplimentare

- [Construirea agenților folosind Model Context Protocol pe Azure](https://learn.microsoft.com/azure/developer/ai/intro-agents-mcp)
- [MCP Remote cu Azure Container Apps (Node.js/TypeScript/JavaScript)](https://learn.microsoft.com/samples/azure-samples/mcp-container-ts/mcp-container-ts/)
- [Agent .NET OpenAI MCP](https://learn.microsoft.com/samples/azure-samples/openai-mcp-agent-dotnet/openai-mcp-agent-dotnet/)

## Ce urmează

Următorul pas: [Începem cu MCP Clients](/03-GettingStarted/02-client/README.md)

**Declinare a responsabilității**:  
Acest document a fost tradus folosind serviciul de traducere AI [Co-op Translator](https://github.com/Azure/co-op-translator). Deși ne străduim pentru acuratețe, vă rugăm să rețineți că traducerile automate pot conține erori sau inexactități. Documentul original în limba sa nativă trebuie considerat sursa autoritară. Pentru informații critice, se recomandă traducerea profesională realizată de un specialist uman. Nu ne asumăm răspunderea pentru eventualele neînțelegeri sau interpretări greșite care pot apărea în urma utilizării acestei traduceri.