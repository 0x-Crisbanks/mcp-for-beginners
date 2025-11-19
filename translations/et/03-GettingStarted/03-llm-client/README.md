<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "4d846ebb88fbb0f00549e2ff8cc3f746",
  "translation_date": "2025-10-11T11:34:51+00:00",
  "source_file": "03-GettingStarted/03-llm-client/README.md",
  "language_code": "et"
}
-->
# Kliendi loomine LLM-iga

Siiani olete näinud, kuidas luua serverit ja klienti. Klient on suutnud serverit selgesõnaliselt kutsuda, et loetleda selle tööriistu, ressursse ja viipasid. Kuid see pole eriti praktiline lähenemine. Teie kasutaja elab agentlikus ajastus ja eeldab, et saab kasutada viipasid ja suhelda LLM-iga. Kasutajale pole oluline, kas te kasutate MCP-d oma võimekuste salvestamiseks, kuid nad ootavad loomuliku keele kasutamist suhtlemiseks. Kuidas me seda lahendame? Lahendus seisneb LLM-i lisamises kliendile.

## Ülevaade

Selles õppetükis keskendume LLM-i lisamisele kliendile ja näitame, kuidas see pakub teie kasutajale palju paremat kogemust.

## Õppe-eesmärgid

Selle õppetüki lõpuks oskate:

- Luua kliendi LLM-iga.
- Sujuvalt suhelda MCP serveriga LLM-i abil.
- Pakkuda paremat kasutajakogemust kliendi poolel.

## Lähenemine

Proovime mõista, millist lähenemist peame rakendama. LLM-i lisamine kõlab lihtsana, kuid kuidas me seda tegelikult teeme?

Siin on, kuidas klient suhtleb serveriga:

1. Loo ühendus serveriga.

1. Loetle võimekused, viipad, ressursid ja tööriistad ning salvesta nende skeem.

1. Lisa LLM ja edasta salvestatud võimekused ja nende skeem formaadis, mida LLM mõistab.

1. Töötle kasutaja viipa, edastades selle LLM-ile koos kliendi loetletud tööriistadega.

Suurepärane, nüüd mõistame, kuidas seda kõrgel tasemel teha. Proovime seda allpool olevas harjutuses.

## Harjutus: Kliendi loomine LLM-iga

Selles harjutuses õpime, kuidas lisada LLM-i oma kliendile.

### Autentimine GitHubi isikliku juurdepääsutokeni abil

GitHubi tokeni loomine on lihtne protsess. Siin on, kuidas seda teha:

- Minge GitHubi seadistustesse – Klõpsake oma profiilipildil paremas ülanurgas ja valige Seadistused.
- Liikuge arendaja seadistustesse – Kerige alla ja klõpsake Arendaja seadistused.
- Valige isiklikud juurdepääsutokenid – Klõpsake peenhäälestatud tokenid ja seejärel Loo uus token.
- Konfigureerige oma token – Lisage viide, määrake aegumiskuupäev ja valige vajalikud ulatused (õigused). Veenduge, et lisate mudelite õiguse.
- Looge ja kopeerige token – Klõpsake Loo token ja veenduge, et kopeerite selle kohe, kuna te ei saa seda hiljem uuesti näha.

### -1- Ühenduse loomine serveriga

Loome esmalt oma kliendi:

#### TypeScript

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StdioClientTransport } from "@modelcontextprotocol/sdk/client/stdio.js";
import { Transport } from "@modelcontextprotocol/sdk/shared/transport.js";
import OpenAI from "openai";
import { z } from "zod"; // Import zod for schema validation

class MCPClient {
    private openai: OpenAI;
    private client: Client;
    constructor(){
        this.openai = new OpenAI({
            baseURL: "https://models.inference.ai.azure.com", 
            apiKey: process.env.GITHUB_TOKEN,
        });

        this.client = new Client(
            {
                name: "example-client",
                version: "1.0.0"
            },
            {
                capabilities: {
                prompts: {},
                resources: {},
                tools: {}
                }
            }
            );    
    }
}
```

Eelnevas koodis oleme:

- Importinud vajalikud teegid.
- Loonud klassi kahe liikmega, `client` ja `openai`, mis aitavad hallata klienti ja suhelda LLM-iga.
- Konfigureerinud LLM-i instantsi GitHubi mudelite kasutamiseks, määrates `baseUrl` viitama inferentsi API-le.

#### Python

```python
from mcp import ClientSession, StdioServerParameters, types
from mcp.client.stdio import stdio_client

# Create server parameters for stdio connection
server_params = StdioServerParameters(
    command="mcp",  # Executable
    args=["run", "server.py"],  # Optional command line arguments
    env=None,  # Optional environment variables
)


async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(
            read, write
        ) as session:
            # Initialize the connection
            await session.initialize()


if __name__ == "__main__":
    import asyncio

    asyncio.run(run())

```

Eelnevas koodis oleme:

- Importinud MCP jaoks vajalikud teegid.
- Loonud kliendi.

#### .NET

```csharp
using Azure;
using Azure.AI.Inference;
using Azure.Identity;
using System.Text.Json;
using ModelContextProtocol.Client;
using ModelContextProtocol.Protocol.Transport;
using System.Text.Json;

var clientTransport = new StdioClientTransport(new()
{
    Name = "Demo Server",
    Command = "/workspaces/mcp-for-beginners/03-GettingStarted/02-client/solution/server/bin/Debug/net8.0/server",
    Arguments = [],
});

await using var mcpClient = await McpClientFactory.CreateAsync(clientTransport);
```

#### Java

Esmalt peate lisama LangChain4j sõltuvused oma `pom.xml` faili. Lisage need sõltuvused MCP integratsiooni ja GitHubi mudelite toe lubamiseks:

```xml
<properties>
    <langchain4j.version>1.0.0-beta3</langchain4j.version>
</properties>

<dependencies>
    <!-- LangChain4j MCP Integration -->
    <dependency>
        <groupId>dev.langchain4j</groupId>
        <artifactId>langchain4j-mcp</artifactId>
        <version>${langchain4j.version}</version>
    </dependency>
    
    <!-- OpenAI Official API Client -->
    <dependency>
        <groupId>dev.langchain4j</groupId>
        <artifactId>langchain4j-open-ai-official</artifactId>
        <version>${langchain4j.version}</version>
    </dependency>
    
    <!-- GitHub Models Support -->
    <dependency>
        <groupId>dev.langchain4j</groupId>
        <artifactId>langchain4j-github-models</artifactId>
        <version>${langchain4j.version}</version>
    </dependency>
    
    <!-- Spring Boot Starter (optional, for production apps) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
```

Seejärel looge oma Java kliendiklass:

```java
import dev.langchain4j.mcp.McpToolProvider;
import dev.langchain4j.mcp.client.DefaultMcpClient;
import dev.langchain4j.mcp.client.McpClient;
import dev.langchain4j.mcp.client.transport.McpTransport;
import dev.langchain4j.mcp.client.transport.http.HttpMcpTransport;
import dev.langchain4j.model.chat.ChatLanguageModel;
import dev.langchain4j.model.openaiofficial.OpenAiOfficialChatModel;
import dev.langchain4j.service.AiServices;
import dev.langchain4j.service.tool.ToolProvider;

import java.time.Duration;
import java.util.List;

public class LangChain4jClient {
    
    public static void main(String[] args) throws Exception {        // Configure the LLM to use GitHub Models
        ChatLanguageModel model = OpenAiOfficialChatModel.builder()
                .isGitHubModels(true)
                .apiKey(System.getenv("GITHUB_TOKEN"))
                .timeout(Duration.ofSeconds(60))
                .modelName("gpt-4.1-nano")
                .build();

        // Create MCP transport for connecting to server
        McpTransport transport = new HttpMcpTransport.Builder()
                .sseUrl("http://localhost:8080/sse")
                .timeout(Duration.ofSeconds(60))
                .logRequests(true)
                .logResponses(true)
                .build();

        // Create MCP client
        McpClient mcpClient = new DefaultMcpClient.Builder()
                .transport(transport)
                .build();
    }
}
```

Eelnevas koodis oleme:

- **Lisanud LangChain4j sõltuvused**: MCP integratsiooni, OpenAI ametliku kliendi ja GitHubi mudelite toe jaoks.
- **Importinud LangChain4j teegid**: MCP integratsiooni ja OpenAI vestlusmudeli funktsionaalsuse jaoks.
- **Loonud `ChatLanguageModel`**: Konfigureeritud GitHubi mudelite kasutamiseks teie GitHubi tokeniga.
- **Seadistanud HTTP transpordi**: Server-Sent Events (SSE) abil MCP serveriga ühenduse loomiseks.
- **Loonud MCP kliendi**: Mis haldab suhtlust serveriga.
- **Kasutanud LangChain4j sisseehitatud MCP tuge**: Mis lihtsustab integratsiooni LLM-ide ja MCP serverite vahel.

#### Rust

See näide eeldab, et teil on Rust-põhine MCP server käimas. Kui teil seda pole, vaadake tagasi [01-esimene-server](../01-first-server/README.md) õppetundi, et server luua.

Kui teil on Rust MCP server, avage terminal ja navigeerige samasse kataloogi kui server. Seejärel käivitage järgmine käsk, et luua uus LLM kliendiprojekt:

```bash
mkdir calculator-llmclient
cd calculator-llmclient
cargo init
```

Lisage järgmised sõltuvused oma `Cargo.toml` faili:

```toml
[dependencies]
async-openai = { version = "0.29.0", features = ["byot"] }
rmcp = { version = "0.5.0", features = ["client", "transport-child-process"] }
serde_json = "1.0.141"
tokio = { version = "1.46.1", features = ["rt-multi-thread"] }
```

> [!NOTE]
> Rusti jaoks pole ametlikku OpenAI teeki, kuid `async-openai` teek on [kogukonna hallatav teek](https://platform.openai.com/docs/libraries/rust#rust), mida sageli kasutatakse.

Avage `src/main.rs` fail ja asendage selle sisu järgmise koodiga:

```rust
use async_openai::{Client, config::OpenAIConfig};
use rmcp::{
    RmcpError,
    model::{CallToolRequestParam, ListToolsResult},
    service::{RoleClient, RunningService, ServiceExt},
    transport::{ConfigureCommandExt, TokioChildProcess},
};
use serde_json::{Value, json};
use std::error::Error;
use tokio::process::Command;

#[tokio::main]
async fn main() -> Result<(), Box<dyn Error>> {
    // Initial message
    let mut messages = vec![json!({"role": "user", "content": "What is the sum of 3 and 2?"})];

    // Setup OpenAI client
    let api_key = std::env::var("OPENAI_API_KEY")?;
    let openai_client = Client::with_config(
        OpenAIConfig::new()
            .with_api_base("https://models.github.ai/inference/chat")
            .with_api_key(api_key),
    );

    // Setup MCP client
    let server_dir = std::path::Path::new(env!("CARGO_MANIFEST_DIR"))
        .parent()
        .unwrap()
        .join("calculator-server");

    let mcp_client = ()
        .serve(
            TokioChildProcess::new(Command::new("cargo").configure(|cmd| {
                cmd.arg("run").current_dir(server_dir);
            }))
            .map_err(RmcpError::transport_creation::<TokioChildProcess>)?,
        )
        .await?;

    // TODO: Get MCP tool listing 

    // TODO: LLM conversation with tool calls

    Ok(())
}
```

See kood seadistab põhitasemel Rusti rakenduse, mis ühendub MCP serveri ja GitHubi mudelitega LLM-i interaktsioonide jaoks.

> [!IMPORTANT]
> Veenduge, et määrate `OPENAI_API_KEY` keskkonnamuutuja oma GitHubi tokeniga enne rakenduse käivitamist.

Suurepärane, järgmiseks sammuks loetleme serveri võimekused.

### -2- Serveri võimekuste loetlemine

Nüüd ühendume serveriga ja küsime selle võimekusi:

#### TypeScript

Samas klassis lisage järgmised meetodid:

```typescript
async connectToServer(transport: Transport) {
     await this.client.connect(transport);
     this.run();
     console.error("MCPClient started on stdin/stdout");
}

async run() {
    console.log("Asking server for available tools");

    // listing tools
    const toolsResult = await this.client.listTools();
}
```

Eelnevas koodis oleme:

- Lisanud koodi serveriga ühenduse loomiseks, `connectToServer`.
- Loonud `run` meetodi, mis vastutab meie rakenduse voo haldamise eest. Praegu loetleb see ainult tööriistad, kuid lisame sellele peagi rohkem.

#### Python

```python
# List available resources
resources = await session.list_resources()
print("LISTING RESOURCES")
for resource in resources:
    print("Resource: ", resource)

# List available tools
tools = await session.list_tools()
print("LISTING TOOLS")
for tool in tools.tools:
    print("Tool: ", tool.name)
    print("Tool", tool.inputSchema["properties"])
```

Siin on, mida me lisasime:

- Ressursside ja tööriistade loetlemine ning nende printimine. Tööriistade puhul loetleme ka `inputSchema`, mida hiljem kasutame.

#### .NET

```csharp
async Task<List<ChatCompletionsToolDefinition>> GetMcpTools()
{
    Console.WriteLine("Listing tools");
    var tools = await mcpClient.ListToolsAsync();

    List<ChatCompletionsToolDefinition> toolDefinitions = new List<ChatCompletionsToolDefinition>();

    foreach (var tool in tools)
    {
        Console.WriteLine($"Connected to server with tools: {tool.Name}");
        Console.WriteLine($"Tool description: {tool.Description}");
        Console.WriteLine($"Tool parameters: {tool.JsonSchema}");

        // TODO: convert tool definition from MCP tool to LLm tool     
    }

    return toolDefinitions;
}
```

Eelnevas koodis oleme:

- Loetlenud MCP serveris saadaval olevad tööriistad.
- Iga tööriista puhul loetlenud nime, kirjelduse ja selle skeemi. Viimane on midagi, mida me kasutame tööriistade kutsumiseks.

#### Java

```java
// Create a tool provider that automatically discovers MCP tools
ToolProvider toolProvider = McpToolProvider.builder()
        .mcpClients(List.of(mcpClient))
        .build();

// The MCP tool provider automatically handles:
// - Listing available tools from the MCP server
// - Converting MCP tool schemas to LangChain4j format
// - Managing tool execution and responses
```

Eelnevas koodis oleme:

- Loonud `McpToolProvider`, mis automaatselt avastab ja registreerib kõik tööriistad MCP serverist.
- Tööriistade pakkuja haldab MCP tööriistade skeemide ja LangChain4j tööriistade formaadi vahelise teisenduse sisemiselt.
- See lähenemine abstraheerib käsitsi tööriistade loetlemise ja teisendamise protsessi.

#### Rust

Tööriistade hankimine MCP serverist toimub `list_tools` meetodi abil. Teie `main` funktsioonis, pärast MCP kliendi seadistamist, lisage järgmine kood:

```rust
// Get MCP tool listing 
let tools = mcp_client.list_tools(Default::default()).await?;
```

### -3- Serveri võimekuste teisendamine LLM-i tööriistadeks

Järgmine samm pärast serveri võimekuste loetlemist on nende teisendamine formaadiks, mida LLM mõistab. Kui me seda teeme, saame pakkuda neid võimekusi tööriistadena LLM-ile.

#### TypeScript

1. Lisage järgmine kood, et teisendada MCP serveri vastus tööriista formaadiks, mida LLM saab kasutada:

    ```typescript
    openAiToolAdapter(tool: {
        name: string;
        description?: string;
        input_schema: any;
        }) {
        // Create a zod schema based on the input_schema
        const schema = z.object(tool.input_schema);
    
        return {
            type: "function" as const, // Explicitly set type to "function"
            function: {
            name: tool.name,
            description: tool.description,
            parameters: {
            type: "object",
            properties: tool.input_schema.properties,
            required: tool.input_schema.required,
            },
            },
        };
    }

    ```

    Ülaltoodud kood võtab MCP serveri vastuse ja teisendab selle tööriista definitsiooni formaadiks, mida LLM mõistab.

1. Uuendame järgmisena `run` meetodit, et loetleda serveri võimekused:

    ```typescript
    async run() {
        console.log("Asking server for available tools");
        const toolsResult = await this.client.listTools();
        const tools = toolsResult.tools.map((tool) => {
            return this.openAiToolAdapter({
            name: tool.name,
            description: tool.description,
            input_schema: tool.inputSchema,
            });
        });
    }
    ```

    Eelnevas koodis oleme uuendanud `run` meetodit, et kaardistada tulemus ja iga kirje puhul kutsuda `openAiToolAdapter`.

#### Python

1. Esiteks loome järgmise teisendaja funktsiooni:

    ```python
    def convert_to_llm_tool(tool):
        tool_schema = {
            "type": "function",
            "function": {
                "name": tool.name,
                "description": tool.description,
                "type": "function",
                "parameters": {
                    "type": "object",
                    "properties": tool.inputSchema["properties"]
                }
            }
        }

        return tool_schema
    ```

    Ülaltoodud funktsioonis `convert_to_llm_tools` võtame MCP tööriista vastuse ja teisendame selle formaadiks, mida LLM mõistab.

1. Järgmisena uuendame oma kliendikoodi, et kasutada seda funktsiooni järgmiselt:

    ```python
    for tool in tools.tools:
        print("Tool: ", tool.name)
        print("Tool", tool.inputSchema["properties"])
        functions.append(convert_to_llm_tool(tool))
    ```

    Siin lisame MCP tööriista vastuse teisendamise LLM-ile edastatavaks formaadiks.

#### .NET

1. Lisame koodi MCP tööriista vastuse teisendamiseks LLM-i mõistetavaks formaadiks:

```csharp
ChatCompletionsToolDefinition ConvertFrom(string name, string description, JsonElement jsonElement)
{ 
    // convert the tool to a function definition
    FunctionDefinition functionDefinition = new FunctionDefinition(name)
    {
        Description = description,
        Parameters = BinaryData.FromObjectAsJson(new
        {
            Type = "object",
            Properties = jsonElement
        },
        new JsonSerializerOptions() { PropertyNamingPolicy = JsonNamingPolicy.CamelCase })
    };

    // create a tool definition
    ChatCompletionsToolDefinition toolDefinition = new ChatCompletionsToolDefinition(functionDefinition);
    return toolDefinition;
}
```

Eelnevas koodis oleme:

- Loonud funktsiooni `ConvertFrom`, mis võtab nime, kirjelduse ja sisendskeemi.
- Määratlenud funktsionaalsuse, mis loob FunctionDefinitioni, mis edastatakse ChatCompletionsDefinitionile. Viimane on midagi, mida LLM mõistab.

1. Vaatame, kuidas saame olemasolevat koodi uuendada, et kasutada ülaltoodud funktsiooni:

    ```csharp
    async Task<List<ChatCompletionsToolDefinition>> GetMcpTools()
    {
        Console.WriteLine("Listing tools");
        var tools = await mcpClient.ListToolsAsync();

        List<ChatCompletionsToolDefinition> toolDefinitions = new List<ChatCompletionsToolDefinition>();

        foreach (var tool in tools)
        {
            Console.WriteLine($"Connected to server with tools: {tool.Name}");
            Console.WriteLine($"Tool description: {tool.Description}");
            Console.WriteLine($"Tool parameters: {tool.JsonSchema}");

            JsonElement propertiesElement;
            tool.JsonSchema.TryGetProperty("properties", out propertiesElement);

            var def = ConvertFrom(tool.Name, tool.Description, propertiesElement);
            Console.WriteLine($"Tool definition: {def}");
            toolDefinitions.Add(def);

            Console.WriteLine($"Properties: {propertiesElement}");        
        }

        return toolDefinitions;
    }
    ```    In the preceding code, we've:

    - Update the function to convert the MCP tool response to an LLm tool. Let's highlight the code we added:

        ```csharp
        JsonElement propertiesElement;
        tool.JsonSchema.TryGetProperty("properties", out propertiesElement);

        var def = ConvertFrom(tool.Name, tool.Description, propertiesElement);
        Console.WriteLine($"Tool definition: {def}");
        toolDefinitions.Add(def);
        ```

        The input schema is part of the tool response but on the "properties" attribute, so we need to extract. Furthermore, we now call `ConvertFrom` with the tool details. Now we've done the heavy lifting, let's see how it call comes together as we handle a user prompt next.

#### Java

```java
// Create a Bot interface for natural language interaction
public interface Bot {
    String chat(String prompt);
}

// Configure the AI service with LLM and MCP tools
Bot bot = AiServices.builder(Bot.class)
        .chatLanguageModel(model)
        .toolProvider(toolProvider)
        .build();
```

Eelnevas koodis oleme:

- Määratlenud lihtsa `Bot` liidese loomuliku keele interaktsioonide jaoks.
- Kasutanud LangChain4j `AiServices`, et automaatselt siduda LLM MCP tööriistade pakkujaga.
- Raamistik haldab automaatselt tööriistade skeemide teisendamist ja funktsioonide kutsumist kulisside taga.
- See lähenemine kõrvaldab käsitsi tööriistade teisendamise - LangChain4j haldab kogu MCP tööriistade LLM-iga ühilduvaks formaadiks teisendamise keerukust.

#### Rust

MCP tööriista vastuse teisendamiseks formaadiks, mida LLM mõistab, lisame abifunktsiooni, mis vormindab tööriistade loetelu. Lisage järgmine kood oma `main.rs` faili `main` funktsiooni alla. Seda kutsutakse LLM-ile päringute tegemisel:

```rust
async fn format_tools(tools: &ListToolsResult) -> Result<Vec<Value>, Box<dyn Error>> {
    let tools_json = serde_json::to_value(tools)?;
    let Some(tools_array) = tools_json.get("tools").and_then(|t| t.as_array()) else {
        return Ok(vec![]);
    };

    let formatted_tools = tools_array
        .iter()
        .filter_map(|tool| {
            let name = tool.get("name")?.as_str()?;
            let description = tool.get("description")?.as_str()?;
            let schema = tool.get("inputSchema")?;

            Some(json!({
                "type": "function",
                "function": {
                    "name": name,
                    "description": description,
                    "parameters": {
                        "type": "object",
                        "properties": schema.get("properties").unwrap_or(&json!({})),
                        "required": schema.get("required").unwrap_or(&json!([]))
                    }
                }
            }))
        })
        .collect();

    Ok(formatted_tools)
}
```

Suurepärane, nüüd oleme valmis kasutajapäringuid käsitlema, nii et tegeleme sellega järgmisena.

### -4- Kasutaja viipa käsitlemine

Selles koodiosas käsitleme kasutajapäringuid.

#### TypeScript

1. Lisage meetod, mida kasutatakse LLM-i kutsumiseks:

    ```typescript
    async callTools(
        tool_calls: OpenAI.Chat.Completions.ChatCompletionMessageToolCall[],
        toolResults: any[]
    ) {
        for (const tool_call of tool_calls) {
        const toolName = tool_call.function.name;
        const args = tool_call.function.arguments;

        console.log(`Calling tool ${toolName} with args ${JSON.stringify(args)}`);


        // 2. Call the server's tool 
        const toolResult = await this.client.callTool({
            name: toolName,
            arguments: JSON.parse(args),
        });

        console.log("Tool result: ", toolResult);

        // 3. Do something with the result
        // TODO  

        }
    }
    ```

    Eelnevas koodis oleme:

    - Lisanud meetodi `callTools`.
    - Meetod võtab LLM-i vastuse ja kontrollib, milliseid tööriistu on kutsutud, kui üldse:

        ```typescript
        for (const tool_call of tool_calls) {
        const toolName = tool_call.function.name;
        const args = tool_call.function.arguments;

        console.log(`Calling tool ${toolName} with args ${JSON.stringify(args)}`);

        // call tool
        }
        ```

    - Kutsutakse tööriista, kui LLM näitab, et seda tuleks kutsuda:

        ```typescript
        // 2. Call the server's tool 
        const toolResult = await this.client.callTool({
            name: toolName,
            arguments: JSON.parse(args),
        });

        console.log("Tool result: ", toolResult);

        // 3. Do something with the result
        // TODO  
        ```

1. Uuendage `run` meetodit, et lisada LLM-i kutsed ja `callTools`:

    ```typescript

    // 1. Create messages that's input for the LLM
    const prompt = "What is the sum of 2 and 3?"

    const messages: OpenAI.Chat.Completions.ChatCompletionMessageParam[] = [
            {
                role: "user",
                content: prompt,
            },
        ];

    console.log("Querying LLM: ", messages[0].content);

    // 2. Calling the LLM
    let response = this.openai.chat.completions.create({
        model: "gpt-4o-mini",
        max_tokens: 1000,
        messages,
        tools: tools,
    });    

    let results: any[] = [];

    // 3. Go through the LLM response,for each choice, check if it has tool calls 
    (await response).choices.map(async (choice: { message: any; }) => {
        const message = choice.message;
        if (message.tool_calls) {
            console.log("Making tool call")
            await this.callTools(message.tool_calls, results);
        }
    });
    ```

Suurepärane, loetleme koodi täies mahus:

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StdioClientTransport } from "@modelcontextprotocol/sdk/client/stdio.js";
import { Transport } from "@modelcontextprotocol/sdk/shared/transport.js";
import OpenAI from "openai";
import { z } from "zod"; // Import zod for schema validation

class MyClient {
    private openai: OpenAI;
    private client: Client;
    constructor(){
        this.openai = new OpenAI({
            baseURL: "https://models.inference.ai.azure.com", // might need to change to this url in the future: https://models.github.ai/inference
            apiKey: process.env.GITHUB_TOKEN,
        });

        this.client = new Client(
            {
                name: "example-client",
                version: "1.0.0"
            },
            {
                capabilities: {
                prompts: {},
                resources: {},
                tools: {}
                }
            }
            );    
    }

    async connectToServer(transport: Transport) {
        await this.client.connect(transport);
        this.run();
        console.error("MCPClient started on stdin/stdout");
    }

    openAiToolAdapter(tool: {
        name: string;
        description?: string;
        input_schema: any;
          }) {
          // Create a zod schema based on the input_schema
          const schema = z.object(tool.input_schema);
      
          return {
            type: "function" as const, // Explicitly set type to "function"
            function: {
              name: tool.name,
              description: tool.description,
              parameters: {
              type: "object",
              properties: tool.input_schema.properties,
              required: tool.input_schema.required,
              },
            },
          };
    }
    
    async callTools(
        tool_calls: OpenAI.Chat.Completions.ChatCompletionMessageToolCall[],
        toolResults: any[]
      ) {
        for (const tool_call of tool_calls) {
          const toolName = tool_call.function.name;
          const args = tool_call.function.arguments;
    
          console.log(`Calling tool ${toolName} with args ${JSON.stringify(args)}`);
    
    
          // 2. Call the server's tool 
          const toolResult = await this.client.callTool({
            name: toolName,
            arguments: JSON.parse(args),
          });
    
          console.log("Tool result: ", toolResult);
    
          // 3. Do something with the result
          // TODO  
    
         }
    }

    async run() {
        console.log("Asking server for available tools");
        const toolsResult = await this.client.listTools();
        const tools = toolsResult.tools.map((tool) => {
            return this.openAiToolAdapter({
              name: tool.name,
              description: tool.description,
              input_schema: tool.inputSchema,
            });
        });

        const prompt = "What is the sum of 2 and 3?";
    
        const messages: OpenAI.Chat.Completions.ChatCompletionMessageParam[] = [
            {
                role: "user",
                content: prompt,
            },
        ];

        console.log("Querying LLM: ", messages[0].content);
        let response = this.openai.chat.completions.create({
            model: "gpt-4o-mini",
            max_tokens: 1000,
            messages,
            tools: tools,
        });    

        let results: any[] = [];
    
        // 1. Go through the LLM response,for each choice, check if it has tool calls 
        (await response).choices.map(async (choice: { message: any; }) => {
          const message = choice.message;
          if (message.tool_calls) {
              console.log("Making tool call")
              await this.callTools(message.tool_calls, results);
          }
        });
    }
    
}

let client = new MyClient();
 const transport = new StdioClientTransport({
            command: "node",
            args: ["./build/index.js"]
        });

client.connectToServer(transport);
```

#### Python

1. Lisame mõned impordid, mida on vaja LLM-i kutsumiseks:

    ```python
    # llm
    import os
    from azure.ai.inference import ChatCompletionsClient
    from azure.ai.inference.models import SystemMessage, UserMessage
    from azure.core.credentials import AzureKeyCredential
    import json
    ```

1. Järgmisena lisame funktsiooni, mis kutsub LLM-i:

    ```python
    # llm

    def call_llm(prompt, functions):
        token = os.environ["GITHUB_TOKEN"]
        endpoint = "https://models.inference.ai.azure.com"

        model_name = "gpt-4o"

        client = ChatCompletionsClient(
            endpoint=endpoint,
            credential=AzureKeyCredential(token),
        )

        print("CALLING LLM")
        response = client.complete(
            messages=[
                {
                "role": "system",
                "content": "You are a helpful assistant.",
                },
                {
                "role": "user",
                "content": prompt,
                },
            ],
            model=model_name,
            tools = functions,
            # Optional parameters
            temperature=1.,
            max_tokens=1000,
            top_p=1.    
        )

        response_message = response.choices[0].message
        
        functions_to_call = []

        if response_message.tool_calls:
            for tool_call in response_message.tool_calls:
                print("TOOL: ", tool_call)
                name = tool_call.function.name
                args = json.loads(tool_call.function.arguments)
                functions_to_call.append({ "name": name, "args": args })

        return functions_to_call
    ```

    Eelnevas koodis oleme:

    - Edastanud meie funktsioonid, mille leidsime MCP serverist ja teisendasime, LLM-ile.
    - Seejärel kutsusime LLM-i nende funktsioonidega.
    - Seejärel kontrollime tulemust, et näha, milliseid funktsioone tuleks kutsuda, kui üldse.
    - Lõpuks edastame funktsioonide massiivi, mida kutsuda.

1. Viimane samm, uuendame oma põhikoodi:

    ```python
    prompt = "Add 2 to 20"

    # ask LLM what tools to all, if any
    functions_to_call = call_llm(prompt, functions)

    # call suggested functions
    for f in functions_to_call:
        result = await session.call_tool(f["name"], arguments=f["args"])
        print("TOOLS result: ", result.content)
    ```

    Seal, see oli viimane samm. Ülaltoodud koodis oleme:

    - Kutsunud MCP tööriista `call_tool` kaudu, kasutades funktsiooni, mida LLM arvas, et peaksime kutsuma vastavalt meie viipale.
    - Printinud tööriista kutse tulemuse MCP serverile.

#### .NET

1. Näitame koodi LLM-i viipa päringu tegemiseks:

    ```csharp
    var tools = await GetMcpTools();

    for (int i = 0; i < tools.Count; i++)
    {
        var tool = tools[i];
        Console.WriteLine($"MCP Tools def: {i}: {tool}");
    }

    // 0. Define the chat history and the user message
    var userMessage = "add 2 and 4";

    chatHistory.Add(new ChatRequestUserMessage(userMessage));

    // 1. Define tools
    ChatCompletionsToolDefinition def = CreateToolDefinition();


    // 2. Define options, including the tools
    var options = new ChatCompletionsOptions(chatHistory)
    {
        Model = "gpt-4o-mini",
        Tools = { tools[0] }
    };

    // 3. Call the model  

    ChatCompletions? response = await client.CompleteAsync(options);
    var content = response.Content;

    ```

    Eelnevas koodis oleme:

    - Hankinud tööriistad MCP serverist, `var tools = await GetMcpTools()`.
    - Määratlenud kasutaja viipa `userMessage`.
    - Koostanud valikute objekti, mis määrab mudeli ja tööriistad.
    - Teinud päringu LLM-ile.

1. Viimane samm, vaatame, kas LLM arvab, et peaksime funktsiooni kutsuma:

    ```csharp
    // 4. Check if the response contains a function call
    ChatCompletionsToolCall? calls = response.ToolCalls.FirstOrDefault();
    for (int i = 0; i < response.ToolCalls.Count; i++)
    {
        var call = response.ToolCalls[i];
        Console.WriteLine($"Tool call {i}: {call.Name} with arguments {call.Arguments}");
        //Tool call 0: add with arguments {"a":2,"b":4}

        var dict = JsonSerializer.Deserialize<Dictionary<string, object>>(call.Arguments);
        var result = await mcpClient.CallToolAsync(
            call.Name,
            dict!,
            cancellationToken: CancellationToken.None
        );

        Console.WriteLine(result.Content.First(c => c.Type == "text").Text);

    }
    ```

    Eelnevas koodis oleme:

    - Läbinud funktsioonikutsumiste loendi.
    - Iga tööriista kutse puhul eraldanud nime ja argumendid ning kutsunud tööriista MCP serveris MCP kliendi abil. Lõpuks printisime tulemused.

Siin on kood täies mahus:

```csharp
using Azure;
using Azure.AI.Inference;
using Azure.Identity;
using System.Text.Json;
using ModelContextProtocol.Client;
using ModelContextProtocol.Protocol.Transport;
using System.Text.Json;

var endpoint = "https://models.inference.ai.azure.com";
var token = Environment.GetEnvironmentVariable("GITHUB_TOKEN"); // Your GitHub Access Token
var client = new ChatCompletionsClient(new Uri(endpoint), new AzureKeyCredential(token));
var chatHistory = new List<ChatRequestMessage>
{
    new ChatRequestSystemMessage("You are a helpful assistant that knows about AI")
};

var clientTransport = new StdioClientTransport(new()
{
    Name = "Demo Server",
    Command = "/workspaces/mcp-for-beginners/03-GettingStarted/02-client/solution/server/bin/Debug/net8.0/server",
    Arguments = [],
});

Console.WriteLine("Setting up stdio transport");

await using var mcpClient = await McpClientFactory.CreateAsync(clientTransport);

ChatCompletionsToolDefinition ConvertFrom(string name, string description, JsonElement jsonElement)
{ 
    // convert the tool to a function definition
    FunctionDefinition functionDefinition = new FunctionDefinition(name)
    {
        Description = description,
        Parameters = BinaryData.FromObjectAsJson(new
        {
            Type = "object",
            Properties = jsonElement
        },
        new JsonSerializerOptions() { PropertyNamingPolicy = JsonNamingPolicy.CamelCase })
    };

    // create a tool definition
    ChatCompletionsToolDefinition toolDefinition = new ChatCompletionsToolDefinition(functionDefinition);
    return toolDefinition;
}



async Task<List<ChatCompletionsToolDefinition>> GetMcpTools()
{
    Console.WriteLine("Listing tools");
    var tools = await mcpClient.ListToolsAsync();

    List<ChatCompletionsToolDefinition> toolDefinitions = new List<ChatCompletionsToolDefinition>();

    foreach (var tool in tools)
    {
        Console.WriteLine($"Connected to server with tools: {tool.Name}");
        Console.WriteLine($"Tool description: {tool.Description}");
        Console.WriteLine($"Tool parameters: {tool.JsonSchema}");

        JsonElement propertiesElement;
        tool.JsonSchema.TryGetProperty("properties", out propertiesElement);

        var def = ConvertFrom(tool.Name, tool.Description, propertiesElement);
        Console.WriteLine($"Tool definition: {def}");
        toolDefinitions.Add(def);

        Console.WriteLine($"Properties: {propertiesElement}");        
    }

    return toolDefinitions;
}

// 1. List tools on mcp server

var tools = await GetMcpTools();
for (int i = 0; i < tools.Count; i++)
{
    var tool = tools[i];
    Console.WriteLine($"MCP Tools def: {i}: {tool}");
}

// 2. Define the chat history and the user message
var userMessage = "add 2 and 4";

chatHistory.Add(new ChatRequestUserMessage(userMessage));


// 3. Define options, including the tools
var options = new ChatCompletionsOptions(chatHistory)
{
    Model = "gpt-4o-mini",
    Tools = { tools[0] }
};

// 4. Call the model  

ChatCompletions? response = await client.CompleteAsync(options);
var content = response.Content;

// 5. Check if the response contains a function call
ChatCompletionsToolCall? calls = response.ToolCalls.FirstOrDefault();
for (int i = 0; i < response.ToolCalls.Count; i++)
{
    var call = response.ToolCalls[i];
    Console.WriteLine($"Tool call {i}: {call.Name} with arguments {call.Arguments}");
    //Tool call 0: add with arguments {"a":2,"b":4}

    var dict = JsonSerializer.Deserialize<Dictionary<string, object>>(call.Arguments);
    var result = await mcpClient.CallToolAsync(
        call.Name,
        dict!,
        cancellationToken: CancellationToken.None
    );

    Console.WriteLine(result.Content.First(c => c.Type == "text").Text);

}

// 5. Print the generic response
Console.WriteLine($"Assistant response: {content}");
```

#### Java

```java
try {
    // Execute natural language requests that automatically use MCP tools
    String response = bot.chat("Calculate the sum of 24.5 and 17.3 using the calculator service");
    System.out.println(response);

    response = bot.chat("What's the square root of 144?");
    System.out.println(response);

    response = bot.chat("Show me the help for the calculator service");
    System.out.println(response);
} finally {
    mcpClient.close();
}
```

Eelnevas koodis oleme:

- Kasutanud lihtsaid loomuliku keele viipasid MCP serveri tööriistadega suhtlemiseks.
- LangChain4j raamistik haldab automaatselt:
  - Kasutaja viipade teisendamist tööriistakutseteks, kui vaja.
  - Sobivate MCP tööriistade kutsumist vastavalt LLM-i otsusele.
  - Vestlusvoo haldamist LLM-i ja MCP serveri vahel.
- `bot.chat()` meetod tagastab loomuliku keele vastused, mis võivad sisaldada MCP tööriistade täitmise tulemusi.
- See lähenemine pakub sujuvat kasutajakogemust, kus kasutajad ei pea teadma MCP rakenduse aluseid.

Täielik koodinäide:

```java
public class LangChain4jClient {
    
    public static void main(String[] args) throws Exception {        ChatLanguageModel model = OpenAiOfficialChatModel.builder()
                .isGitHubModels(true)
                .apiKey(System.getenv("GITHUB_TOKEN"))
                .timeout(Duration.ofSeconds(60))
                .modelName("gpt-4.1-nano")
                .timeout(Duration.ofSeconds(60))
                .build();

        McpTransport transport = new HttpMcpTransport.Builder()
                .sseUrl("http://localhost:8080/sse")
                .timeout(Duration.ofSeconds(60))
                .logRequests(true)
                .logResponses(true)
                .build();

        McpClient mcpClient = new DefaultMcpClient.Builder()
                .transport(transport)
                .build();

        ToolProvider toolProvider = McpToolProvider.builder()
                .mcpClients(List.of(mcpClient))
                .build();

        Bot bot = AiServices.builder(Bot.class)
                .chatLanguageModel(model)
                .toolProvider(toolProvider)
                .build();

        try {
            String response = bot.chat("Calculate the sum of 24.5 and 17.3 using the calculator service");
            System.out.println(response);

            response = bot.chat("What's the square root of 144?");
            System.out.println(response);

            response = bot.chat("Show me the help for the calculator service");
            System.out.println(response);
        } finally {
            mcpClient.close();
        }
    }
}
```

#### Rust

Siin toimub suurem osa tööst. Kutsume LLM-i algse kasutaja viipaga, seejärel töötleme vastust, et näha, kas mõnda tööriista tuleb kutsuda. Kui jah, kutsume need tööriistad ja jätkame vestlust LLM-iga, kuni pole vaja rohkem tööriistakutseid ja meil on lõplik vastus.

Teeme LLM-ile mitu päringut, seega määratleme funktsiooni, mis haldab LLM-i kutset. Lisage järgmine funktsioon oma `main.rs` faili:

```rust
async fn call_llm(
    client: &Client<OpenAIConfig>,
    messages: &[Value],
    tools: &ListToolsResult,
) -> Result<Value, Box<dyn Error>> {
    let response = client
        .completions()
        .create_byot(json!({
            "messages": messages,
            "model": "openai/gpt-4.1",
            "tools": format_tools(tools).await?,
        }))
        .await?;
    Ok(response)
}
```

See funktsioon võtab LLM-i kliendi, sõnumite loendi (sealhulgas kasutaja viipa), MCP serveri tööriistad ja saadab päringu LLM-ile, tagastades vastuse.
LLM-i vastus sisaldab massiivi `choices`. Me peame töötlema tulemust, et näha, kas seal on `tool_calls`. See annab meile teada, et LLM soovib, et konkreetne tööriist kutsutaks argumentidega. Lisa järgmine kood oma `main.rs` faili lõppu, et defineerida funktsioon LLM-i vastuse käsitlemiseks:

```rust
async fn process_llm_response(
    llm_response: &Value,
    mcp_client: &RunningService<RoleClient, ()>,
    openai_client: &Client<OpenAIConfig>,
    mcp_tools: &ListToolsResult,
    messages: &mut Vec<Value>,
) -> Result<(), Box<dyn Error>> {
    let Some(message) = llm_response
        .get("choices")
        .and_then(|c| c.as_array())
        .and_then(|choices| choices.first())
        .and_then(|choice| choice.get("message"))
    else {
        return Ok(());
    };

    // Print content if available
    if let Some(content) = message.get("content").and_then(|c| c.as_str()) {
        println!("🤖 {}", content);
    }

    // Handle tool calls
    if let Some(tool_calls) = message.get("tool_calls").and_then(|tc| tc.as_array()) {
        messages.push(message.clone()); // Add assistant message

        // Execute each tool call
        for tool_call in tool_calls {
            let (tool_id, name, args) = extract_tool_call_info(tool_call)?;
            println!("⚡ Calling tool: {}", name);

            let result = mcp_client
                .call_tool(CallToolRequestParam {
                    name: name.into(),
                    arguments: serde_json::from_str::<Value>(&args)?.as_object().cloned(),
                })
                .await?;

            // Add tool result to messages
            messages.push(json!({
                "role": "tool",
                "tool_call_id": tool_id,
                "content": serde_json::to_string_pretty(&result)?
            }));
        }

        // Continue conversation with tool results
        let response = call_llm(openai_client, messages, mcp_tools).await?;
        Box::pin(process_llm_response(
            &response,
            mcp_client,
            openai_client,
            mcp_tools,
            messages,
        ))
        .await?;
    }
    Ok(())
}
```

Kui `tool_calls` on olemas, eraldab see tööriista teabe, kutsub MCP serverit tööriista päringuga ja lisab tulemused vestluse sõnumitesse. Seejärel jätkab vestlust LLM-iga ning sõnumid uuendatakse assistendi vastuse ja tööriista kutse tulemustega.

Et eraldada tööriista kutse teavet, mida LLM tagastab MCP päringute jaoks, lisame veel ühe abifunktsiooni, et eraldada kõik vajalikud andmed päringu tegemiseks. Lisa järgmine kood oma `main.rs` faili lõppu:

```rust
fn extract_tool_call_info(tool_call: &Value) -> Result<(String, String, String), Box<dyn Error>> {
    let tool_id = tool_call
        .get("id")
        .and_then(|id| id.as_str())
        .unwrap_or("")
        .to_string();
    let function = tool_call.get("function").ok_or("Missing function")?;
    let name = function
        .get("name")
        .and_then(|n| n.as_str())
        .unwrap_or("")
        .to_string();
    let args = function
        .get("arguments")
        .and_then(|a| a.as_str())
        .unwrap_or("{}")
        .to_string();
    Ok((tool_id, name, args))
}
```

Kui kõik osad on paigas, saame nüüd käsitleda algset kasutaja päringut ja kutsuda LLM-i. Uuenda oma `main` funktsiooni, et lisada järgmine kood:

```rust
// LLM conversation with tool calls
let response = call_llm(&openai_client, &messages, &tools).await?;
process_llm_response(
    &response,
    &mcp_client,
    &openai_client,
    &tools,
    &mut messages,
)
.await?;
```

See pärib LLM-i algse kasutaja päringuga, küsides kahe arvu summat, ja töötleb vastust, et dünaamiliselt käsitleda tööriista kutseid.

Suurepärane, sa tegid selle ära!

## Ülesanne

Võta harjutuse kood ja ehita server välja veel mõne tööriistaga. Seejärel loo klient LLM-iga, nagu harjutuses, ja testi seda erinevate päringutega, et veenduda, et kõik sinu serveri tööriistad kutsutakse dünaamiliselt. Selline kliendi ehitamise viis tagab lõppkasutajale suurepärase kasutuskogemuse, kuna nad saavad kasutada päringuid, mitte täpseid kliendi käske, ja jäävad MCP serveri kutsumisest teadmatuks.

## Lahendus

[Lahendus](/03-GettingStarted/03-llm-client/solution/README.md)

## Olulised punktid

- LLM-i lisamine kliendile pakub paremat viisi MCP serveritega suhtlemiseks.
- MCP serveri vastus tuleb teisendada millekski, mida LLM mõistab.

## Näited

- [Java Kalkulaator](../samples/java/calculator/README.md)
- [.Net Kalkulaator](../../../../03-GettingStarted/samples/csharp)
- [JavaScript Kalkulaator](../samples/javascript/README.md)
- [TypeScript Kalkulaator](../samples/typescript/README.md)
- [Python Kalkulaator](../../../../03-GettingStarted/samples/python)
- [Rust Kalkulaator](../../../../03-GettingStarted/samples/rust)

## Lisamaterjalid

## Mis edasi

- Järgmine: [Serveri kasutamine Visual Studio Code'iga](../04-vscode/README.md)

---

**Lahtiütlus**:  
See dokument on tõlgitud AI tõlketeenuse [Co-op Translator](https://github.com/Azure/co-op-translator) abil. Kuigi püüame tagada täpsust, palume arvestada, et automaatsed tõlked võivad sisaldada vigu või ebatäpsusi. Algne dokument selle algses keeles tuleks pidada autoriteetseks allikaks. Olulise teabe puhul soovitame kasutada professionaalset inimtõlget. Me ei vastuta selle tõlke kasutamisest tulenevate arusaamatuste või valesti tõlgenduste eest.