<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "9191921de355cd9c8f46ebe21bdd52fd",
  "translation_date": "2025-06-13T00:05:05+00:00",
  "source_file": "03-GettingStarted/README.md",
  "language_code": "da"
}
-->
## Kom godt i gang

Denne sektion består af flere lektioner:

- **1 Din første server**, i denne første lektion lærer du, hvordan du opretter din første server og inspicerer den med inspector-værktøjet, en værdifuld måde at teste og fejlfinde din server på, [til lektionen](/03-GettingStarted/01-first-server/README.md)

- **2 Client**, i denne lektion lærer du at skrive en client, der kan forbinde til din server, [til lektionen](/03-GettingStarted/02-client/README.md)

- **3 Client med LLM**, en endnu bedre måde at skrive en client på er ved at tilføje en LLM, så den kan "forhandle" med din server om, hvad der skal gøres, [til lektionen](/03-GettingStarted/03-llm-client/README.md)

- **4 Forbrug af en server GitHub Copilot Agent-tilstand i Visual Studio Code**. Her ser vi på at køre vores MCP Server fra Visual Studio Code, [til lektionen](/03-GettingStarted/04-vscode/README.md)

- **5 Forbrug fra en SSE (Server Sent Events)** SSE er en standard for server-til-client streaming, som gør det muligt for servere at sende realtidsopdateringer til klienter over HTTP [til lektionen](/03-GettingStarted/05-sse-server/README.md)

- **6 HTTP Streaming med MCP (Streamable HTTP)**. Lær om moderne HTTP-streaming, fremdriftsnotifikationer og hvordan man implementerer skalerbare, realtids MCP-servere og clients ved hjælp af Streamable HTTP. [til lektionen](/03-GettingStarted/06-http-streaming/README.md)

- **7 Udnyttelse af AI Toolkit for VSCode** til at forbruge og teste dine MCP Clients og Servers [til lektionen](/03-GettingStarted/07-aitk/README.md)

- **8 Testning**. Her fokuserer vi især på, hvordan vi kan teste vores server og client på forskellige måder, [til lektionen](/03-GettingStarted/08-testing/README.md)

- **9 Udrulning**. Dette kapitel ser på forskellige måder at udrulle dine MCP-løsninger på, [til lektionen](/03-GettingStarted/09-deployment/README.md)


Model Context Protocol (MCP) er en åben protokol, der standardiserer, hvordan applikationer leverer kontekst til LLM’er. Tænk på MCP som en USB-C port for AI-applikationer – den giver en standardiseret måde at forbinde AI-modeller til forskellige datakilder og værktøjer.

## Læringsmål

Når du har gennemført denne lektion, vil du kunne:

- Opsætte udviklingsmiljøer for MCP i C#, Java, Python, TypeScript og JavaScript
- Bygge og udrulle grundlæggende MCP-servere med brugerdefinerede funktioner (ressourcer, prompts og værktøjer)
- Oprette host-applikationer, der forbinder til MCP-servere
- Teste og fejlfinde MCP-implementeringer
- Forstå almindelige opsætningsudfordringer og deres løsninger
- Forbinde dine MCP-implementeringer til populære LLM-tjenester

## Opsætning af dit MCP-miljø

Før du går i gang med MCP, er det vigtigt at forberede dit udviklingsmiljø og forstå den grundlæggende arbejdsproces. Denne sektion guider dig gennem de indledende opsætningsskridt for at sikre en glidende start med MCP.

### Forudsætninger

Før du dykker ned i MCP-udvikling, skal du sikre dig, at du har:

- **Udviklingsmiljø**: Til dit valgte sprog (C#, Java, Python, TypeScript eller JavaScript)
- **IDE/Editor**: Visual Studio, Visual Studio Code, IntelliJ, Eclipse, PyCharm eller en anden moderne kodeeditor
- **Pakkehåndteringer**: NuGet, Maven/Gradle, pip eller npm/yarn
- **API-nøgler**: Til de AI-tjenester, du planlægger at bruge i dine host-applikationer

### Officielle SDK’er

I de kommende kapitler vil du se løsninger bygget med Python, TypeScript, Java og .NET. Her er alle de officielt understøttede SDK’er.

MCP tilbyder officielle SDK’er til flere sprog:
- [C# SDK](https://github.com/modelcontextprotocol/csharp-sdk) - Vedligeholdt i samarbejde med Microsoft
- [Java SDK](https://github.com/modelcontextprotocol/java-sdk) - Vedligeholdt i samarbejde med Spring AI
- [TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk) - Den officielle TypeScript-implementering
- [Python SDK](https://github.com/modelcontextprotocol/python-sdk) - Den officielle Python-implementering
- [Kotlin SDK](https://github.com/modelcontextprotocol/kotlin-sdk) - Den officielle Kotlin-implementering
- [Swift SDK](https://github.com/modelcontextprotocol/swift-sdk) - Vedligeholdt i samarbejde med Loopwork AI
- [Rust SDK](https://github.com/modelcontextprotocol/rust-sdk) - Den officielle Rust-implementering

## Vigtige pointer

- Opsætning af et MCP-udviklingsmiljø er enkelt med sprog-specifikke SDK’er
- At bygge MCP-servere indebærer at oprette og registrere værktøjer med klare skemaer
- MCP-klienter forbinder til servere og modeller for at udnytte udvidede funktioner
- Test og fejlretning er afgørende for pålidelige MCP-implementeringer
- Udrulningsmuligheder spænder fra lokal udvikling til cloud-baserede løsninger

## Øvelse

Vi har et sæt eksempler, der supplerer de øvelser, du vil se i alle kapitler i denne sektion. Derudover har hvert kapitel også sine egne øvelser og opgaver.

- [Java Calculator](./samples/java/calculator/README.md)
- [.Net Calculator](../../../03-GettingStarted/samples/csharp)
- [JavaScript Calculator](./samples/javascript/README.md)
- [TypeScript Calculator](./samples/typescript/README.md)
- [Python Calculator](../../../03-GettingStarted/samples/python)

## Yderligere ressourcer

- [Build Agents using Model Context Protocol on Azure](https://learn.microsoft.com/azure/developer/ai/intro-agents-mcp)
- [Remote MCP with Azure Container Apps (Node.js/TypeScript/JavaScript)](https://learn.microsoft.com/samples/azure-samples/mcp-container-ts/mcp-container-ts/)
- [.NET OpenAI MCP Agent](https://learn.microsoft.com/samples/azure-samples/openai-mcp-agent-dotnet/openai-mcp-agent-dotnet/)

## Hvad nu?

Næste: [Opret din første MCP Server](/03-GettingStarted/01-first-server/README.md)

**Ansvarsfraskrivelse**:  
Dette dokument er oversat ved hjælp af AI-oversættelsestjenesten [Co-op Translator](https://github.com/Azure/co-op-translator). Selvom vi bestræber os på nøjagtighed, bedes du være opmærksom på, at automatiserede oversættelser kan indeholde fejl eller unøjagtigheder. Det oprindelige dokument på dets modersmål bør betragtes som den autoritative kilde. For kritisk information anbefales professionel menneskelig oversættelse. Vi påtager os intet ansvar for misforståelser eller fejltolkninger, der måtte opstå som følge af brugen af denne oversættelse.