<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "4d846ebb88fbb0f00549e2ff8cc3f746",
  "translation_date": "2025-10-11T11:33:57+00:00",
  "source_file": "03-GettingStarted/03-llm-client/README.md",
  "language_code": "ta"
}
-->
# LLM மூலம் கிளையன்ட் உருவாக்குதல்

இ bisher, நீங்கள் ஒரு சர்வர் மற்றும் ஒரு கிளையன்ட் உருவாக்குவது எப்படி என்பதை பார்த்தீர்கள். கிளையன்ட் சர்வரை அழைத்து அதன் கருவிகள், வளங்கள் மற்றும் ப்ராம்ப்ட்களை பட்டியலிட முடிந்தது. ஆனால், இது மிகவும் நடைமுறைமான அணுகுமுறை அல்ல. உங்கள் பயனர் ஏஜென்டிக் காலத்தில் வாழ்கிறார், மற்றும் ப்ராம்ப்ட்களை பயன்படுத்தி LLM உடன் தொடர்பு கொள்ள விரும்புகிறார். உங்கள் பயனர் MCP ஐ உங்கள் திறன்களை சேமிக்க பயன்படுத்துகிறீர்களா என்பதை கவலைப்படமாட்டார், ஆனால் இயற்கை மொழியை பயன்படுத்தி தொடர்பு கொள்ள எதிர்பார்க்கிறார்கள். இதை எப்படி தீர்க்கலாம்? தீர்வு என்பது கிளையன்டில் LLM ஐ சேர்ப்பது பற்றியது.

## மேற்பார்வை

இந்த பாடத்தில், உங்கள் கிளையன்டில் LLM ஐ சேர்ப்பது மற்றும் இது உங்கள் பயனருக்கு எவ்வாறு சிறந்த அனுபவத்தை வழங்குகிறது என்பதை பார்க்கிறோம்.

## கற்றல் நோக்கங்கள்

இந்த பாடத்தின் முடிவில், நீங்கள்:

- LLM உடன் ஒரு கிளையன்ட் உருவாக்க முடியும்.
- MCP சர்வரை LLM மூலம் எளிதாக தொடர்பு கொள்ள முடியும்.
- கிளையன்ட் பக்கம் பயனர் அனுபவத்தை மேம்படுத்த முடியும்.

## அணுகுமுறை

நாம் எடுக்க வேண்டிய அணுகுமுறையை புரிந்துகொள்ள முயற்சிப்போம். LLM ஐ சேர்ப்பது எளிதாக தோன்றுகிறது, ஆனால் நாங்கள் உண்மையில் இதை செய்வோமா?

இங்கே கிளையன்ட் சர்வருடன் தொடர்பு கொள்ளும் முறை:

1. சர்வருடன் இணைப்பு ஏற்படுத்தவும்.

1. திறன்கள், ப்ராம்ப்ட்கள், வளங்கள் மற்றும் கருவிகளை பட்டியலிட்டு அவற்றின் ஸ்கீமாவை சேமிக்கவும்.

1. LLM ஐ சேர்த்து, சேமிக்கப்பட்ட திறன்கள் மற்றும் அவற்றின் ஸ்கீமாவை LLM புரிந்துகொள்ளும் வடிவத்தில் அனுப்பவும்.

1. பயனர் ப்ராம்ப்டை LLM க்கு அனுப்பி, கிளையன்ட் பட்டியலிட்ட கருவிகளுடன் சேர்த்து செயல்படுத்தவும்.

சிறந்தது, இப்போது நாம் இதை உயர் மட்டத்தில் எப்படி செய்யலாம் என்பதை புரிந்துகொண்டோம், கீழே உள்ள பயிற்சியில் இதை முயற்சிப்போம்.

## பயிற்சி: LLM உடன் கிளையன்ட் உருவாக்குதல்

இந்த பயிற்சியில், உங்கள் கிளையன்டில் LLM ஐ சேர்ப்பதைக் கற்றுக்கொள்வோம்.

### GitHub Personal Access Token மூலம் அங்கீகாரம்

GitHub டோக்கனை உருவாக்குவது நேர்மையான செயல்முறையாகும். இதை எப்படி செய்வது:

- GitHub அமைப்புகள் செல்லவும் – உங்கள் சுயவிவரப் படத்தை மேலே வலது மூலையில் கிளிக் செய்து அமைப்புகளைத் தேர்ந்தெடுக்கவும்.
- Developer Settings செல்லவும் – கீழே ஸ்க்ரோல் செய்து Developer Settings ஐ கிளிக் செய்யவும்.
- Personal Access Tokens ஐ தேர்ந்தெடுக்கவும் – Fine-grained tokens ஐ கிளிக் செய்து புதிய டோக்கனை உருவாக்கவும்.
- உங்கள் டோக்கனை அமைக்கவும் – குறிப்புக்குறிப்பு சேர்க்கவும், காலாவதியாகும் தேதியை அமைக்கவும், தேவையான ஸ்கோப்புகளை (அனுமதிகள்) தேர்ந்தெடுக்கவும். இந்த வழக்கில் Models அனுமதியை சேர்க்கவும்.
- டோக்கனை உருவாக்கி நகலெடுக்கவும் – Generate token ஐ கிளிக் செய்யவும், அதை உடனடியாக நகலெடுக்கவும், ஏனெனில் அதை மீண்டும் பார்க்க முடியாது.

### -1- சர்வருடன் இணைப்பு

முதலில் உங்கள் கிளையன்டை உருவாக்குவோம்:

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

மேலே உள்ள குறியீட்டில் நாம்:

- தேவையான நூலகங்களை இறக்குமதி செய்துள்ளோம்.
- `client` மற்றும் `openai` என்ற இரண்டு உறுப்புகளுடன் ஒரு வகுப்பை உருவாக்கியுள்ளோம், இது கிளையன்டை நிர்வகிக்கவும், LLM உடன் தொடர்பு கொள்ளவும் உதவும்.
- `baseUrl` ஐ inference API ஐ சுட்டிக்காட்ட LLM instance ஐ GitHub Models ஐ பயன்படுத்த அமைத்துள்ளோம்.

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

மேலே உள்ள குறியீட்டில் நாம்:

- MCP க்கான தேவையான நூலகங்களை இறக்குமதி செய்துள்ளோம்.
- ஒரு கிளையன்டை உருவாக்கியுள்ளோம்.

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

முதலில், உங்கள் `pom.xml` கோப்பில் LangChain4j சார்புகளை சேர்க்க வேண்டும். MCP ஒருங்கிணைப்பு மற்றும் GitHub Models ஆதரவை இயக்க இந்த சார்புகளைச் சேர்க்கவும்:

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

பின்னர் உங்கள் Java கிளையன்ட் வகுப்பை உருவாக்கவும்:

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

மேலே உள்ள குறியீட்டில் நாம்:

- **LangChain4j சார்புகளைச் சேர்த்துள்ளோம்**: MCP ஒருங்கிணைப்பு, OpenAI அதிகாரப்பூர்வ கிளையன்ட் மற்றும் GitHub Models ஆதரவை இயக்க.
- **LangChain4j நூலகங்களை இறக்குமதி செய்துள்ளோம்**: MCP ஒருங்கிணைப்பு மற்றும் OpenAI உரையாடல் மாடல் செயல்பாட்டிற்காக.
- **`ChatLanguageModel` ஐ உருவாக்கியுள்ளோம்**: உங்கள் GitHub டோக்கனுடன் GitHub Models ஐ பயன்படுத்த அமைத்துள்ளோம்.
- **HTTP போக்குவரத்தை அமைத்துள்ளோம்**: MCP சர்வருடன் இணைக்க Server-Sent Events (SSE) ஐ பயன்படுத்த.
- **MCP கிளையன்டை உருவாக்கியுள்ளோம்**: இது சர்வருடன் தொடர்பு கொள்ளும்.
- **LangChain4j இன் உள்ளமைக்கப்பட்ட MCP ஆதரவை பயன்படுத்தியுள்ளோம்**: இது LLM கள் மற்றும் MCP சர்வர்களுக்கு இடையிலான ஒருங்கிணைப்பை எளிமைப்படுத்துகிறது.

#### Rust

இந்த எடுத்துக்காட்டு Rust அடிப்படையிலான MCP சர்வர் இயங்குகிறது என்று கருதுகிறது. MCP சர்வர் இல்லையெனில், [01-first-server](../01-first-server/README.md) பாடத்தைப் பார்த்து சர்வரை உருவாக்கவும்.

Rust MCP சர்வர் இருந்தால், ஒரு டெர்மினலை திறந்து சர்வர் உள்ள கோப்பகத்திற்குச் செல்லவும். பின்னர் புதிய LLM கிளையன்ட் திட்டத்தை உருவாக்க கீழே உள்ள கட்டளையை இயக்கவும்:

```bash
mkdir calculator-llmclient
cd calculator-llmclient
cargo init
```

உங்கள் `Cargo.toml` கோப்பில் கீழே உள்ள சார்புகளைச் சேர்க்கவும்:

```toml
[dependencies]
async-openai = { version = "0.29.0", features = ["byot"] }
rmcp = { version = "0.5.0", features = ["client", "transport-child-process"] }
serde_json = "1.0.141"
tokio = { version = "1.46.1", features = ["rt-multi-thread"] }
```

> [!NOTE]
> OpenAI க்கான அதிகாரப்பூர்வ Rust நூலகம் இல்லை, ஆனால் `async-openai` கிரேட் [சமூக பராமரிப்பு நூலகம்](https://platform.openai.com/docs/libraries/rust#rust) ஆகும், இது பொதுவாக பயன்படுத்தப்படுகிறது.

`src/main.rs` கோப்பைத் திறந்து அதன் உள்ளடக்கத்தை கீழே உள்ள குறியீட்டுடன் மாற்றவும்:

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

இந்த குறியீடு MCP சர்வர் மற்றும் GitHub Models உடன் LLM தொடர்புகளுக்கு இணைக்கும் அடிப்படை Rust பயன்பாட்டை அமைக்கிறது.

> [!IMPORTANT]
> பயன்பாட்டை இயக்குவதற்கு முன் `OPENAI_API_KEY` சூழல் மாறியை உங்கள் GitHub டோக்கனுடன் அமைக்கவும்.

சிறந்தது, அடுத்த படியாக சர்வரில் திறன்களை பட்டியலிடுவோம்.

### -2- சர்வர் திறன்களை பட்டியலிடவும்

இப்போது சர்வருடன் இணைந்து அதன் திறன்களை கேட்கலாம்:

#### TypeScript

அதே வகுப்பில், கீழே உள்ள முறைகளைச் சேர்க்கவும்:

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

மேலே உள்ள குறியீட்டில் நாம்:

- சர்வருடன் இணைக்க `connectToServer` என்ற முறையைச் சேர்த்துள்ளோம்.
- எங்கள் பயன்பாட்டு ஓட்டத்தை நிர்வகிக்க பொறுப்பான `run` முறையை உருவாக்கியுள்ளோம். இதுவரை இது கருவிகளை மட்டுமே பட்டியலிடுகிறது, ஆனால் விரைவில் மேலும் சேர்ப்போம்.

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

இங்கே நாம் சேர்த்துள்ளோம்:

- வளங்கள் மற்றும் கருவிகளை பட்டியலிட்டு அச்சிட்டுள்ளோம். கருவிகளுக்கு, பின்னர் பயன்படுத்த `inputSchema` ஐ பட்டியலிடுகிறோம்.

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

மேலே உள்ள குறியீட்டில் நாம்:

- MCP சர்வரில் கிடைக்கும் கருவிகளை பட்டியலிட்டுள்ளோம்.
- ஒவ்வொரு கருவிக்கும், பெயர், விளக்கம் மற்றும் அதன் ஸ்கீமாவை பட்டியலிட்டுள்ளோம். பின்னர் கருவிகளை அழைக்க இதை பயன்படுத்துவோம்.

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

மேலே உள்ள குறியீட்டில் நாம்:

- MCP சர்வரிலிருந்து அனைத்து கருவிகளையும் தானாக கண்டறிந்து பதிவு செய்ய `McpToolProvider` ஐ உருவாக்கியுள்ளோம்.
- கருவி வழங்குநர் MCP கருவி ஸ்கீமாக்கள் மற்றும் LangChain4j கருவி வடிவம் இடையேயான மாற்றத்தை உள்ளடகமாக நிர்வகிக்கிறது.
- இந்த அணுகுமுறை கருவி பட்டியலிடல் மற்றும் மாற்ற செயல்முறையை கையேடு செய்யாமல் விலக்குகிறது.

#### Rust

MCP சர்வரிலிருந்து கருவிகளை பெற `list_tools` முறையைப் பயன்படுத்தலாம். MCP கிளையன்டை அமைத்த பிறகு, உங்கள் `main` செயல்பாட்டில் கீழே உள்ள குறியீட்டைச் சேர்க்கவும்:

```rust
// Get MCP tool listing 
let tools = mcp_client.list_tools(Default::default()).await?;
```

### -3- சர்வர் திறன்களை LLM கருவிகளாக மாற்றவும்

சர்வர் திறன்களை பட்டியலிடும் அடுத்த படி, அவற்றை LLM புரிந்துகொள்ளும் வடிவமாக மாற்ற வேண்டும். இதைச் செய்த பிறகு, இந்த திறன்களை LLM க்கு கருவிகளாக வழங்கலாம்.

#### TypeScript

1. MCP சர்வரிலிருந்து பதிலை LLM பயன்படுத்தக்கூடிய கருவி வரையறை வடிவமாக மாற்ற கீழே உள்ள குறியீட்டைச் சேர்க்கவும்:

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

    மேலே உள்ள குறியீடு MCP சர்வரிலிருந்து பதிலை எடுத்து, LLM புரிந்துகொள்ளும் கருவி வரையறை வடிவமாக மாற்றுகிறது.

1. அடுத்ததாக `run` முறையைப் புதுப்பிக்கவும்:

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

    மேலே உள்ள குறியீட்டில், பதிலின் மூலம் வரைபடம் உருவாக்கி, ஒவ்வொரு பதிவுக்கும் `openAiToolAdapter` ஐ அழைக்கிறோம்.

#### Python

1. முதலில், கீழே உள்ள மாற்றி செயல்பாட்டை உருவாக்குவோம்:

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

    மேலே உள்ள `convert_to_llm_tools` செயல்பாட்டில், MCP கருவி பதிலை எடுத்து, LLM புரிந்துகொள்ளும் வடிவமாக மாற்றுகிறோம்.

1. அடுத்ததாக, இந்த செயல்பாட்டை பயன்படுத்த உங்கள் கிளையன்ட் குறியீட்டை புதுப்பிக்கவும்:

    ```python
    for tool in tools.tools:
        print("Tool: ", tool.name)
        print("Tool", tool.inputSchema["properties"])
        functions.append(convert_to_llm_tool(tool))
    ```

    இங்கே, MCP கருவி பதிலை LLM க்கு வழங்கக்கூடிய வடிவமாக மாற்ற `convert_to_llm_tool` ஐ அழைக்கிறோம்.

#### .NET

1. MCP கருவி பதிலை LLM புரிந்துகொள்ளும் வடிவமாக மாற்ற குறியீட்டைச் சேர்க்கவும்:

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

மேலே உள்ள குறியீட்டில் நாம்:

- `ConvertFrom` என்ற செயல்பாட்டை உருவாக்கியுள்ளோம், இது பெயர், விளக்கம் மற்றும் உள்ளீட்டு ஸ்கீமாவை எடுக்கிறது.
- FunctionDefinition ஐ உருவாக்கும் செயல்பாட்டை வரையறுத்துள்ளோம், இது ChatCompletionsDefinition க்கு அனுப்பப்படுகிறது. இது LLM புரிந்துகொள்ளும் ஒன்று.

1. மேலே உள்ள செயல்பாட்டை பயன்படுத்த சில உள்ளமைக்கப்பட்ட குறியீட்டை புதுப்பிக்க எப்படி என்பதைப் பார்ப்போம்:

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

மேலே உள்ள குறியீட்டில் நாம்:

- இயற்கை மொழி தொடர்புகளுக்கு ஒரு எளிய `Bot` இடைமுகத்தை வரையறுத்துள்ளோம்.
- LLM ஐ MCP கருவி வழங்குநருடன் தானாக இணைக்க LangChain4j இன் `AiServices` ஐ பயன்படுத்தியுள்ளோம்.
- கட்டமைப்பு MCP கருவிகளை LLM-இன் இணக்கமான வடிவமாக மாற்றும் சிக்கல்களை தானாக நிர்வகிக்கிறது.
- இந்த அணுகுமுறை MCP கருவிகளை LLM-இன் இணக்கமான வடிவமாக மாற்ற LangChain4j அனைத்து சிக்கல்களையும் நிர்வகிக்கிறது.

#### Rust

MCP கருவி பதிலை LLM புரிந்துகொள்ளும் வடிவமாக மாற்ற, கருவி பட்டியலிடலை வடிவமைக்கும் உதவியாளர் செயல்பாட்டைச் சேர்க்க வேண்டும். LLM க்கு கோரிக்கைகளைச் செய்யும்போது இது அழைக்கப்படும். உங்கள் `main.rs` கோப்பில் `main` செயல்பாட்டின் கீழே இந்த குறியீட்டைச் சேர்க்கவும்:

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

சிறந்தது, பயனர் கோரிக்கைகளை நிர்வகிக்க அமைக்கப்பட்டுள்ளோம், அடுத்ததாக அதைச் செய்யலாம்.

### -4- பயனர் ப்ராம்ப்ட் கோரிக்கையை நிர்வகிக்கவும்

இந்த குறியீட்டில், பயனர் கோரிக்கைகளை நிர்வகிக்கிறோம்.

#### TypeScript

1. எங்கள் LLM ஐ அழைக்க பயன்படுத்தப்படும் முறையைச் சேர்க்கவும்:

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

    மேலே உள்ள குறியீட்டில் நாம்:

    - `callTools` என்ற முறையைச் சேர்த்துள்ளோம்.
    - LLM பதிலை எடுத்து, எந்த கருவிகள் அழைக்கப்பட்டுள்ளன என்பதைச் சரிபார்க்கிறது:

        ```typescript
        for (const tool_call of tool_calls) {
        const toolName = tool_call.function.name;
        const args = tool_call.function.arguments;

        console.log(`Calling tool ${toolName} with args ${JSON.stringify(args)}`);

        // call tool
        }
        ```

    - LLM குறிப்பிடும் கருவியை அழைக்கிறது:

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

1. `run` முறையை LLM அழைப்புகளைச் சேர்க்கவும்:

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

சிறந்தது, முழு குறியீட்டை பட்டியலிடுவோம்:

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

1. LLM ஐ அழைக்க தேவையான இறக்குமதிகளைச் சேர்க்கவும்:

    ```python
    # llm
    import os
    from azure.ai.inference import ChatCompletionsClient
    from azure.ai.inference.models import SystemMessage, UserMessage
    from azure.core.credentials import AzureKeyCredential
    import json
    ```

1. அடுத்ததாக, LLM ஐ அழைக்கும் செயல்பாட்டைச் சேர்க்கவும்:

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

    மேலே உள்ள குறியீட்டில் நாம்:

    - MCP சர்வரில் கண்டுபிடிக்கப்பட்ட மற்றும் மாற்றப்பட்ட செயல்பாடுகளை LLM க்கு அனுப்பியுள்ளோம்.
    - பின்னர், LLM ஐ அந்த செயல்பாடுகளுடன் அழைத்துள்ளோம்.
    - பின்னர், எந்த செயல்பாடுகளை அழைக்க வேண்டும் என்பதைப் பார்க்கிறோம்.
    - இறுதியாக, அழைக்க வேண்டிய செயல்பாடுகளின் வரிசையை அனுப்புகிறோம்.

1. இறுதி படி, எங்கள் முக்கிய குறியீட்டை புதுப்பிக்கவும்:

    ```python
    prompt = "Add 2 to 20"

    # ask LLM what tools to all, if any
    functions_to_call = call_llm(prompt, functions)

    # call suggested functions
    for f in functions_to_call:
        result = await session.call_tool(f["name"], arguments=f["args"])
        print("TOOLS result: ", result.content)
    ```

    மேலே உள்ள குறியீட்டில் நாம்:

    - MCP சர்வரில் `call_tool` மூலம் MCP கருவியை அழைக்கிறோம்.
    - MCP சர்வரில் கருவி அழைப்பின் முடிவுகளை அச்சிடுகிறோம்.

#### .NET

1. LLM ப்ராம்ப்ட் கோரிக்கையைச் செய்ய சில குறியீட்டைச் சேர்க்கவும்:

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

    மேலே உள்ள குறியீட்டில் நாம்:

    - MCP சர்வரில் இருந்து கருவிகளை பெற்றுள்ளோம், `var tools = await GetMcpTools()`.
    - பயனர் ப்ராம்ப்ட் `userMessage` ஐ வரையறுத்துள்ளோம்.
    - மாடல் மற்றும் கருவிகளை குறிப்பிடும் விருப்பங்கள் பொருளை உருவாக்கியுள்ளோம்.
    - LLM க்கு கோரிக்கை செய்துள்ளோம்.

1. ஒரு செயல்பாட்டை அழைக்க LLM யோசிக்கிறதா என்பதைப் பார்ப்போம்:

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

    மேலே உள்ள குறியீட்டில் நாம்:

    - செயல்பாட்டு அழைப்புகளின் பட்டியலின் மூலம் மடக்குகிறோம்.
    - ஒவ்வொரு கருவி அழைப்பிற்கும், பெயர் மற்றும் வாதங்களைப் பிரித்து MCP சர்வரில் MCP கிளையன்டைப் பயன்படுத்தி கருவியை அழைக்கிறோம். இறுதியாக முடிவுகளை அச்சிடுகிறோம்.

முழு குறியீடு:

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

மேலே உள்ள குறியீட்டில் நாம்:

- MCP சர்வர் கருவிகளுடன் தொடர்பு கொள்ள எளிய இயற்கை மொழி ப்ராம்ப்ட்களைப் பயன்படுத்தியுள்ளோம்.
- LangChain4j கட்டமைப்பு தானாக நிர்வகிக்கிறது:
  - தேவையான போது பயனர் ப்ராம்ப்ட்களை கருவி அழைப்புகளாக மாற்றுகிறது.
  - LLM இன் முடிவின் அடிப்படையில் சரியான MCP கருவிகளை அழைக்கிறது.
  - LLM மற்றும் MCP சர்வருக்கு இடையேயான உரையாடல் ஓட்டத்தை நிர்வகிக்கிறது.
- `bot.chat()` முறை MCP கருவி செயல்பாடுகளின் முடிவுகளை உள்ளடக்கக்கூடிய இயற்கை மொழி பதில்களைத் திருப்புகிறது.
- இந்த அணுகுமுறை MCP செயல்பாட்டின் அடிப்படையில் பயனர் MCP செயல்பாட்டை அறிய தேவையில்லை.

முழு குறியீட்டு எடுத்துக்காட்டு:

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

இங்கே பெரும்பாலான வேலைகள் நடக்கின்றன. ஆரம்ப பயனர் ப்ராம்ப்டுடன் LLM ஐ அழைக்கிறோம், பின்னர் எந்த கருவிகளை அழைக்க வேண்டும் என்பதைப் பார்க்கிறோம். அவற்றை அழைக்க வேண்டும் என்றால், அந்த கருவிகளை அழைக்கிறோம் மற்றும் எந்த கருவி அழைப்புகளும் தேவையில்லை மற்றும் இறுதி பதில் கிடைக்கும் வரை LLM உடன் உரையாடலைத் தொடருகிறோம்.

LLM க்கு பல அழைப்புகளைச் செய்யவுள்ளோம், எனவே LLM அழைப்பை நிர்வகிக்கும் செயல்பாட்டை வரையறுக்கலாம். உங்கள் `main.rs` கோப்பில் கீழே உள்ள செயல்பாட்டைச் சேர்க்கவும்:

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

இந்த செயல்பாடு LLM கிளையன்டை, செய்திகளின் பட்டியலை (பயனர் ப்ராம்ப்டை உட்படுத்தி), MCP சர்வரில் இருந்து கருவிகளை எடுத்து, LLM க்கு கோரிக்கையை அனுப்புகிறது மற்றும் பதிலை திருப்புகிறது.
LLM பதிலில் `choices` என்ற வரிசை இருக்கும். `tool_calls` உள்ளதா என்பதைப் பார்க்க முடிவுகளை செயலாக்க வேண்டும். இது LLM ஒரு குறிப்பிட்ட கருவியை வாதங்களுடன் அழைக்க வேண்டும் என்பதை நமக்கு தெரிவிக்கிறது. LLM பதிலை கையாள ஒரு செயல்பாட்டை வரையறுக்க உங்கள் `main.rs` கோப்பின் அடியில் பின்வரும் குறியீட்டை சேர்க்கவும்:

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

`tool_calls` இருந்தால், அது கருவி தகவல்களை எடுக்கும், MCP சர்வரை கருவி கோரிக்கையுடன் அழைக்கும், மற்றும் உரையாடல் செய்திகளுக்கு முடிவுகளைச் சேர்க்கும். பின்னர் LLM உடன் உரையாடலைத் தொடர்கிறது, மேலும் உதவியாளரின் பதில் மற்றும் கருவி அழைப்பு முடிவுகளுடன் செய்திகள் புதுப்பிக்கப்படும்.

MCP அழைப்புகளுக்காக LLM திருப்பும் கருவி அழைப்பு தகவல்களை எடுக்க, அழைப்பைச் செய்ய தேவையான அனைத்தையும் எடுக்க மற்றொரு உதவியாளர் செயல்பாட்டைச் சேர்க்கப் போகிறோம். உங்கள் `main.rs` கோப்பின் அடியில் பின்வரும் குறியீட்டைச் சேர்க்கவும்:

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

எல்லா பகுதிகளும் இடத்தில் உள்ளதால், ஆரம்ப பயனர் உத்தேசத்தை கையாளவும் LLM ஐ அழைக்கவும் முடியும். உங்கள் `main` செயல்பாட்டை பின்வரும் குறியீட்டை உள்ளடக்க புதுப்பிக்கவும்:

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

இது இரண்டு எண்களின் கூட்டத்தை கேட்கும் ஆரம்ப பயனர் உத்தேசத்துடன் LLM ஐ விசாரிக்கும், மேலும் கருவி அழைப்புகளை தன்னிச்சையாக கையாள பதிலை செயலாக்கும்.

சிறப்பாக செய்துவிட்டீர்கள்!

## பணிக்குறிப்பு

பயிற்சியில் உள்ள குறியீட்டை எடுத்து, மேலும் சில கருவிகளுடன் சர்வரை உருவாக்கவும். பின்னர் பயிற்சியில் உள்ளதைப் போலவே LLM உடன் ஒரு கிளையண்டை உருவாக்கி, உங்கள் சர்வர் கருவிகள் அனைத்தும் தன்னிச்சையாக அழைக்கப்படுகிறதா என்பதை உறுதிப்படுத்த பல்வேறு உத்தேசங்களுடன் சோதிக்கவும். இந்த வகையான கிளையண்ட் கட்டுமானம் இறுதி பயனாளருக்கு சிறந்த பயனர் அனுபவத்தை வழங்கும், ஏனெனில் அவர்கள் குறிப்பிட்ட கிளையண்ட் கட்டளைகளைப் பயன்படுத்தாமல், MCP சர்வர் அழைக்கப்படுவதை அறியாமல் உத்தேசங்களைப் பயன்படுத்த முடியும்.

## தீர்வு

[தீர்வு](/03-GettingStarted/03-llm-client/solution/README.md)

## முக்கிய குறிப்புகள்

- உங்கள் கிளையண்டுடன் LLM ஐச் சேர்ப்பது MCP சர்வர்களுடன் தொடர்பு கொள்ள பயனர்களுக்கு சிறந்த வழியை வழங்குகிறது.
- MCP சர்வர் பதிலை LLM புரிந்துகொள்ளும் வகையில் மாற்ற வேண்டும்.

## மாதிரிகள்

- [Java Calculator](../samples/java/calculator/README.md)
- [.Net Calculator](../../../../03-GettingStarted/samples/csharp)
- [JavaScript Calculator](../samples/javascript/README.md)
- [TypeScript Calculator](../samples/typescript/README.md)
- [Python Calculator](../../../../03-GettingStarted/samples/python)
- [Rust Calculator](../../../../03-GettingStarted/samples/rust)

## கூடுதல் ஆதாரங்கள்

## அடுத்தது என்ன

- அடுத்தது: [Visual Studio Code ஐப் பயன்படுத்தி ஒரு சர்வரை நுகருதல்](../04-vscode/README.md)

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையைப் பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. நாங்கள் துல்லியத்திற்காக முயற்சிக்கிறோம், ஆனால் தானியக்க மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறான தகவல்கள் இருக்கக்கூடும் என்பதை தயவுசெய்து கவனத்தில் கொள்ளுங்கள். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.