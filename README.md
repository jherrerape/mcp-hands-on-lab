# 🧪 Taller Práctico: Primeros Pasos con Model Context Protocol (MCP)

### Aprende MCP construyendo un servidor, un cliente y un cliente con LLM — en C# y Python

[![MCP](https://img.shields.io/badge/Model%20Context%20Protocol-MCP-blueviolet)](https://modelcontextprotocol.io/)
[![C#](https://img.shields.io/badge/C%23-.NET%208%2B-512BD4)](https://dotnet.microsoft.com/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB)](https://www.python.org/)
[![Basado en](https://img.shields.io/badge/Basado%20en-microsoft%2Fmcp--for--beginners-181717?logo=github)](https://github.com/microsoft/mcp-for-beginners)

---

## 🧠 Acerca de este taller

Este taller es una adaptación reducida y enfocada del curso oficial [**MCP for Beginners**](https://github.com/microsoft/mcp-for-beginners) de Microsoft. El curso original cubre seis lenguajes (C#, Java, JavaScript, Python, Rust y TypeScript); esta guía se concentra **únicamente en C# y Python** y en los tres primeros laboratorios prácticos del Módulo 3, que son la puerta de entrada al desarrollo con MCP:

| # | Laboratorio | Qué construirás |
|---|--------------|------------------|
| 1 | **First Server** | Un servidor MCP tipo "calculadora" con una herramienta (*tool*) y un recurso (*resource*) |
| 2 | **First Client** | Un cliente que se conecta al servidor, lista sus capacidades y las invoca de forma explícita |
| 3 | **Client with LLM** | Un cliente que conecta un LLM (vía GitHub Models) para que decida, en lenguaje natural, qué herramientas del servidor invocar |

> 💡 El *Model Context Protocol (MCP)* es un protocolo abierto que estandariza cómo las aplicaciones proveen contexto (datos, herramientas, prompts) a los modelos de lenguaje. Piensa en MCP como un "puerto USB-C" para aplicaciones de IA: una forma única de conectar cualquier modelo con cualquier herramienta o fuente de datos.

---

## 📋 Tabla de contenido

- [Prerrequisitos](#-prerrequisitos)
- [Verificación rápida del entorno](#-verificación-rápida-del-entorno)
- [Estructura del taller](#-estructura-del-taller)
- [Lab 1 · First Server](#-lab-1--creando-tu-primer-servidor-mcp-first-server)
- [Lab 2 · First Client](#-lab-2--creando-tu-primer-cliente-mcp-first-client)
- [Lab 3 · Client with LLM](#-lab-3--cliente-con-llm-client-with-llm)
- [Solución de problemas comunes](#-solución-de-problemas-comunes)
- [Recursos adicionales](#-recursos-adicionales)
- [Créditos y licencia](#-créditos-y-licencia)

---

## ✅ Prerrequisitos

Antes de comenzar, asegúrate de tener listo tu entorno de desarrollo. Divide los requisitos en **generales** (aplican sin importar el lenguaje que elijas) y **específicos por lenguaje**.

### 🔧 Requisitos generales (obligatorios para ambos lenguajes)

| Requisito | Detalle | Por qué lo necesitas |
|---|---|---|
| **Cuenta de GitHub** | Gratuita, en [github.com](https://github.com) | Necesaria para generar un *Personal Access Token* con permiso **Models**, usado en el Lab 3 para llamar al LLM a través de GitHub Models |
| **Node.js LTS (v18 o superior) + npm** | [nodejs.org](https://nodejs.org/) | El **MCP Inspector**, la herramienta gráfica que usarás para probar tus servidores, se ejecuta como paquete de Node vía `npx`, independientemente del lenguaje del servidor |
| **Git** | [git-scm.com](https://git-scm.com/) | Para clonar el repositorio de referencia y versionar tu propio código |
| **Editor de código** | VS Code recomendado, con las extensiones *C# Dev Kit* y *Python* | Autocompletado, depuración y ejecución de proyectos |
| **Terminal / línea de comandos** | La de tu sistema operativo o la integrada en VS Code | Para ejecutar todos los comandos de este taller |
| **Conexión a internet** | — | Para descargar paquetes (NuGet/pip) y llamar a la API de GitHub Models en el Lab 3 |

> Para cada herramienta de abajo, primero ejecuta el comando de verificación. **Si ya te muestra un número de versión que cumple el mínimo, no necesitas instalar nada**: pasa directo a la siguiente herramienta.

#### 📥 Git

**🔎 Verifica si ya lo tienes:**

```bash
git --version
```

Si el comando no se reconoce, instálalo según tu sistema operativo:

**🪟 Windows**

1. Descarga el instalador desde [git-scm.com/download/win](https://git-scm.com/download/win).
2. Ejecuta el instalador y deja las opciones por defecto (incluye "Git Bash" y "Add Git to PATH").
3. Abre una **nueva** terminal y vuelve a correr `git --version` para confirmar.

**🍎 macOS**

Opción recomendada, con [Homebrew](https://brew.sh/):

```bash
brew install git
```

Alternativa: instala las *Command Line Tools* de Xcode, que incluyen Git:

```bash
xcode-select --install
```

---

#### 📥 Node.js LTS y npm

**🔎 Verifica si ya lo tienes:**

```bash
node --version
npm --version
```

Necesitas Node **v18 o superior**. Si no lo tienes o la versión es menor, instálalo:

**🪟 Windows**

1. Descarga el instalador **LTS** desde [nodejs.org](https://nodejs.org/).
2. Ejecuta el instalador con las opciones por defecto (npm se instala junto con Node).
3. Abre una **nueva** terminal y vuelve a correr `node --version` para confirmar.

**🍎 macOS**

```bash
brew install node
```

> 💡 También puedes usar [nvm-windows](https://github.com/coreybutler/nvm-windows) en Windows o [nvm](https://github.com/nvm-sh/nvm) en macOS si necesitas manejar varias versiones de Node.

---

#### 📥 Visual Studio Code (opcional pero recomendado)

**🔎 Verifica si ya lo tienes:**

```bash
code --version
```

Si no lo tienes, instálalo (mismo proceso en **Windows** y **macOS**):

1. Descarga el instalador desde [code.visualstudio.com](https://code.visualstudio.com/).
2. Instálalo con las opciones por defecto.
3. Abre VS Code y ve a la pestaña **Extensions** (`Ctrl+Shift+X` en Windows, `Cmd+Shift+X` en macOS) e instala:
   - **C# Dev Kit** (para los labs en C#)
   - **Python** (extensión oficial de Microsoft, para los labs en Python)

---

#### 📥 Cuenta y token de GitHub

1. Si no tienes cuenta, créala gratis en [github.com/join](https://github.com/join).
2. La generación del **Personal Access Token** con permiso **Models** se explica en detalle en la sección [Lab 3 · Obtener un token de GitHub](#-obtener-un-token-de-github-github-models), ya que solo se necesita en ese momento del taller.

---

### 🟣 Requisitos específicos — C#

| Requisito | Versión mínima | Detalle |
|---|---|---|
| **.NET SDK** | .NET 8.0 o superior | Incluye el `dotnet` CLI y NuGet |
| Paquetes NuGet que instalarás durante el taller | `ModelContextProtocol` (versión `1.4.1`), `Microsoft.Extensions.Hosting`, `Azure.AI.Inference`, `Azure.Identity` | Se instalan con `dotnet add package` en cada lab, no ahora |

#### 📥 .NET SDK

**🔎 Verifica si ya lo tienes:**

```bash
dotnet --version
```

Necesitas la versión **8.0 o superior**. Si el comando no se reconoce o la versión es menor, instálalo:

**🪟 Windows**

1. Descarga el instalador del **.NET SDK 8.0 (o superior)** desde [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download).
2. Ejecuta el instalador `.exe` y sigue el asistente con las opciones por defecto.
3. Abre una **nueva** terminal y vuelve a correr `dotnet --version` para confirmar.

**🍎 macOS**

Opción con Homebrew:

```bash
brew install --cask dotnet-sdk
```

O descarga el instalador `.pkg` desde [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download) y ejecútalo.

> Los paquetes NuGet (`ModelContextProtocol`, `Microsoft.Extensions.Hosting`, `Azure.AI.Inference`, `Azure.Identity`) **no se instalan ahora**: los agregarás con `dotnet add package` dentro de cada proyecto, en el paso correspondiente de cada lab. Usa siempre la versión estable `1.4.1` de `ModelContextProtocol`, sin la bandera `--prerelease`.

---

### 🐍 Requisitos específicos — Python

| Requisito | Versión mínima | Detalle |
|---|---|---|
| **Python** | 3.10 o superior | Incluye `pip` y el módulo `venv` |
| Paquetes que instalarás durante el taller | `mcp[cli]`, `azure-ai-inference`, `azure-core` | Se instalan con `pip install` dentro de un entorno virtual, en cada lab |

#### 📥 Python y pip

**🔎 Verifica si ya lo tienes:**

```bash
python --version
pip --version
```

> En macOS es posible que debas usar `python3` y `pip3` en lugar de `python`/`pip`.

Necesitas Python **3.10 o superior**. Si no lo tienes o la versión es menor, instálalo:

**🪟 Windows**

1. Descarga el instalador desde [python.org/downloads](https://www.python.org/downloads/).
2. **Importante**: marca la casilla **"Add python.exe to PATH"** en la primera pantalla del instalador.
3. Haz clic en **Install Now** y espera a que finalice.
4. Abre una **nueva** terminal y vuelve a correr `python --version` para confirmar.

**🍎 macOS**

Opción recomendada, con Homebrew:

```bash
brew install python
```

macOS trae una versión de Python del sistema, pero se recomienda usar la de Homebrew para evitar conflictos.

> Los paquetes de Python (`mcp[cli]`, `azure-ai-inference`, `azure-core`) tampoco se instalan globalmente: cada lab indica cuándo crear el entorno virtual (`venv`) e instalarlos con `pip install` dentro de él.

---

### 🔍 Verificación final del entorno

Ya revisaste cada herramienta por separado arriba. Como último chequeo, copia y ejecuta todo junto en tu terminal:

```bash
# Herramientas generales
git --version
node --version
npm --version

# Rama C#
dotnet --version

# Rama Python
python --version
pip --version
```

Si algún comando falla o muestra una versión inferior a la indicada arriba, instala o actualiza esa herramienta antes de continuar.

---

## 🗺️ Estructura del taller

```
mcp-hands-on-lab/
├── lab-1-first-server/
│   ├── csharp/
│   └── python/
├── lab-2-first-client/
│   ├── csharp/
│   └── python/
└── lab-3-client-with-llm/
    ├── csharp/
    └── python/
```

Cada laboratorio es independiente por lenguaje: puedes seguir la ruta completa en C#, la ruta completa en Python, o ambas para comparar los dos enfoques.

---

## 🔨 Lab 1 · Creando tu primer servidor MCP (First Server)

### Objetivos de aprendizaje

- Entender qué es un servidor MCP y qué puede exponer (herramientas, recursos, prompts).
- Crear un servidor MCP mínimo tipo calculadora.
- Ejecutarlo y probarlo con **MCP Inspector**.

### 🧩 ¿Qué compone un servidor MCP?

- **Tools (herramientas)**: funciones que el modelo puede invocar (ej. `add`).
- **Resources (recursos)**: datos de contexto que el servidor expone (ej. un saludo dinámico).
- **Prompts**: plantillas reutilizables de texto.

---

### 🟣 C#

**Paso 1 — Crear el proyecto**

```bash
dotnet new console -n McpCalculatorServer
cd McpCalculatorServer
```

**Paso 2 — Agregar los paquetes necesarios**

```bash
dotnet add package ModelContextProtocol --version 1.4.1
dotnet add package Microsoft.Extensions.Hosting
```

> ⚠️ **No uses la bandera `--prerelease`.** Esa bandera instala la versión experimental más reciente del SDK, que puede implementar cambios del protocolo (como el campo `resultType`) que el MCP Inspector todavía no reconoce, y verás un error de conexión con `"unrecognized_keys"`. La versión `1.4.1` es estable y compatible con el Inspector.

**Paso 3 — Escribir el servidor**

Reemplaza el contenido de `Program.cs`:

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using ModelContextProtocol.Server;
using System.ComponentModel;

var builder = Host.CreateApplicationBuilder(args);

// Los logs deben ir a stderr: stdout se reserva para el protocolo MCP
builder.Logging.AddConsole(options =>
{
    options.LogToStandardErrorThreshold = LogLevel.Trace;
});

builder.Services
    .AddMcpServer()
    .WithStdioServerTransport()
    .WithToolsFromAssembly();

await builder.Build().RunAsync();

[McpServerToolType]
public static class CalculatorTool
{
    [McpServerTool, Description("Suma dos números")]
    public static string Add(int a, int b) => $"Resultado: {a + b}";
}
```

**Paso 4 — Compilar y ejecutar el servidor**

```bash
dotnet build
dotnet run
```

Verifica que se quede "colgado" esperando entrada (eso es correcto, un servidor stdio no imprime nada). Detén con `Ctrl+C` antes del siguiente paso.

**Paso 5 — Probarlo con MCP Inspector**

Tienes tres formas equivalentes de apuntar el Inspector a tu servidor. Usa la que prefieras:

| Forma | Comando | Cuándo usarla |
|---|---|---|
| **Vía `dotnet run`** | `npx @modelcontextprotocol/inspector dotnet run` | La más simple; recompila si hay cambios pendientes |
| **Vía `--project` (.csproj)** | `npx @modelcontextprotocol/inspector dotnet run --project McpCalculatorServer.csproj` | Útil si corres el comando desde otra carpeta |
| **Vía el ejecutable (.exe) ya compilado** | `npx @modelcontextprotocol/inspector bin\Debug\net8.0\McpCalculatorServer.exe` | La más rápida: arranca directo, sin pasar por `dotnet build` cada vez |

> 💡 Para usar la opción del `.exe`, corre primero `dotnet build` (Paso 4) para generarlo. La ruta puede variar según tu versión del SDK; confírmala con `dir bin\Debug\net8.0\*.exe`.

Esto abrirá una interfaz web local. Conéctate, selecciona **Tools → Add**, ingresa valores para `a` y `b`, y verifica el resultado.

---

### 🐍 Python

**Paso 1 — Crear el proyecto y el entorno virtual**

```bash
mkdir calculator-server
cd calculator-server
python -m venv venv
```

Activa el entorno:

```bash
# Windows
venv\Scripts\activate

# macOS
source venv/bin/activate
```

**Paso 2 — Instalar el SDK de MCP**

```bash
pip install "mcp[cli]"
```

**Paso 3 — Escribir el servidor**

Crea `server.py`:

```python
from mcp.server.fastmcp import FastMCP

# Crear el servidor MCP
mcp = FastMCP("Calculadora")

@mcp.tool()
def add(a: int, b: int) -> int:
    """Suma dos números"""
    return a + b

@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Genera un saludo personalizado"""
    return f"¡Hola, {name}!"

if __name__ == "__main__":
    mcp.run()
```

**Paso 4 — Ejecutar el servidor**

```bash
mcp run server.py
```

**Paso 5 — Probarlo con MCP Inspector**

```bash
npx @modelcontextprotocol/inspector mcp run server.py
```

> 💡 `mcp dev server.py` también abre el Inspector automáticamente, pero no soporta todos los métodos; se recomienda el comando de arriba con `npx`.

---

### ✅ Puntos clave del Lab 1

- Configurar un entorno MCP es sencillo gracias a los SDKs oficiales por lenguaje.
- Un servidor se compone principalmente de *tools* y *resources* con esquemas bien definidos.
- El MCP Inspector es la forma más rápida de validar que tu servidor funciona antes de escribir un cliente.

---

## 🔌 Lab 2 · Creando tu primer cliente MCP (First Client)

### Objetivos de aprendizaje

- Entender qué hace un cliente MCP.
- Escribir un cliente que se conecte al servidor del Lab 1.
- Listar e invocar explícitamente las capacidades del servidor (sin LLM todavía).

### 🧩 ¿Qué necesita un cliente?

- Importar las librerías del SDK cliente.
- Instanciar el cliente y conectarlo a un *transport* (usaremos `stdio`, pensado para procesos locales).
- Listar tools/resources/prompts.
- Invocarlos con los argumentos correctos.

---

### 🟣 C#

**Paso 1 — Crear el proyecto cliente**

```bash
dotnet new console -n McpCalculatorClient
cd McpCalculatorClient
dotnet add package ModelContextProtocol --version 1.4.1
```

**Paso 2 — Escribir el cliente**

El `StdioClientTransport` necesita saber **cómo arrancar el servidor**. Tienes dos formas equivalentes de indicarlo — elige una:

**Opción A — Apuntando al `.csproj` (recompila cada vez, útil mientras desarrollas):**

```csharp
using ModelContextProtocol.Client;
using ModelContextProtocol.Protocol;

// El transport stdio inicia el servidor como subproceso y habla con él por stdin/stdout
var clientTransport = new StdioClientTransport(new()
{
    Name = "Servidor Calculadora",
    Command = "dotnet",
    Arguments = ["run", "--project", "../McpCalculatorServer/McpCalculatorServer.csproj"],
});
```

**Opción B — Apuntando directo al `.exe` ya compilado (arranca más rápido, ideal una vez que el servidor está estable):**

```csharp
using ModelContextProtocol.Client;
using ModelContextProtocol.Protocol;

var clientTransport = new StdioClientTransport(new()
{
    Name = "Servidor Calculadora",
    Command = @"..\McpCalculatorServer\bin\Debug\net8.0\McpCalculatorServer.exe",
    Arguments = [],
});
```

> 💡 Para usar la Opción B, recuerda correr `dotnet build` dentro de `McpCalculatorServer` al menos una vez antes de ejecutar el cliente, y ajustar la ruta si tu versión del SDK generó otra carpeta (`net8.0`, `net9.0`, etc.).

El resto del código es igual para ambas opciones:

```csharp
await using var mcpClient = await McpClient.CreateAsync(clientTransport);

// Listar herramientas disponibles
Console.WriteLine("Herramientas disponibles:");
foreach (var tool in await mcpClient.ListToolsAsync())
{
    Console.WriteLine($"- {tool.Name}: {tool.Description}");
}

// Invocar la herramienta "Add"
var result = await mcpClient.CallToolAsync(
    "Add",
    new Dictionary<string, object?> { ["a"] = 5, ["b"] = 7 },
    cancellationToken: CancellationToken.None);

// "Content" es una colección de tipos polimórficos (TextContentBlock, ImageContentBlock, etc.),
// por eso filtramos con OfType<TextContentBlock>() para acceder a la propiedad "Text"
Console.WriteLine(result.Content.OfType<TextContentBlock>().First().Text);
```

**Paso 3 — Ejecutar el cliente**

```bash
dotnet run
```

El cliente arrancará el servidor automáticamente como subproceso (usando la opción A o B que hayas elegido), listará sus herramientas y ejecutará `Add(5, 7)`.

---

### 🐍 Python

**Paso 1 — Crear el cliente**

En la misma carpeta que `server.py` (o en el mismo entorno virtual), crea `client.py`:

```python
import asyncio
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

# Parámetros para lanzar el servidor como subproceso
server_params = StdioServerParameters(
    command="mcp",
    args=["run", "server.py"],
    env=None,
)

async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()

            # Listar recursos
            resources = await session.list_resources()
            print("RECURSOS DISPONIBLES")
            for resource in resources:
                print("Recurso:", resource)

            # Listar herramientas
            tools = await session.list_tools()
            print("HERRAMIENTAS DISPONIBLES")
            for tool in tools.tools:
                print("Herramienta:", tool.name)

            # Invocar la herramienta "add"
            result = await session.call_tool("add", arguments={"a": 5, "b": 7})
            print("Resultado:", result.content)

if __name__ == "__main__":
    asyncio.run(run())
```

**Paso 2 — Ejecutar el cliente**

```bash
python client.py
```

---

### ✅ Puntos clave del Lab 2

- Un cliente puede iniciar al servidor él mismo (como en este lab) o conectarse a uno ya corriendo.
- Listar e invocar capacidades manualmente es el paso previo indispensable antes de delegarle esa decisión a un LLM.
- Es una alternativa a Inspector para pruebas automatizadas o integradas en otro flujo.

---

## 🤖 Lab 3 · Cliente con LLM (Client with LLM)

Hasta ahora el cliente decide manualmente qué herramienta invocar. En este laboratorio delegamos esa decisión a un **LLM**: el usuario escribe una instrucción en lenguaje natural y el modelo decide, de forma dinámica, si debe llamar a alguna herramienta del servidor MCP.

```
Usuario ──prompt en lenguaje natural──▶  Cliente
                                            │
                             1. Lista tools del servidor MCP
                             2. Convierte tools → formato del LLM
                             3. Envía prompt + tools al LLM
                             4. Si el LLM pide una tool → la invoca en el servidor MCP
                             5. Devuelve la respuesta final al usuario
```

### Objetivos de aprendizaje

- Añadir un LLM al cliente MCP.
- Convertir el esquema de las herramientas del servidor a un formato que el LLM entienda.
- Cerrar el ciclo: prompt → decisión del LLM → llamada a la herramienta → respuesta.

### 🔑 Obtener un token de GitHub (GitHub Models)

Este laboratorio usa **GitHub Models** como proveedor de LLM (gratuito para pruebas, compatible con la API de OpenAI):

1. Entra a **GitHub → Settings** (clic en tu foto de perfil, arriba a la derecha).
2. Ve a **Developer settings**.
3. Selecciona **Personal access tokens → Fine-grained tokens → Generate new token**.
4. Ponle un nombre, define una fecha de expiración y en los permisos (*scopes*) habilita **Models**.
5. Genera el token y **cópialo de inmediato** (no podrás verlo de nuevo).
6. Guárdalo como variable de entorno:

```bash
# macOS
export GITHUB_TOKEN="tu_token_aqui"

# Windows (PowerShell)
$env:GITHUB_TOKEN="tu_token_aqui"
```

---

### 🟣 C#

**Paso 1 — Agregar paquetes**

```bash
dotnet add package Azure.AI.Inference
dotnet add package Azure.Identity
```

**Paso 2 — Escribir el cliente con LLM**

```csharp
using Azure;
using Azure.AI.Inference;
using System.Text.Json;
using ModelContextProtocol.Client;
using ModelContextProtocol.Protocol;

var endpoint = "https://models.inference.ai.azure.com";
var token = Environment.GetEnvironmentVariable("GITHUB_TOKEN");
var chatClient = new ChatCompletionsClient(new Uri(endpoint), new AzureKeyCredential(token));

var chatHistory = new List<ChatRequestMessage>
{
    new ChatRequestSystemMessage("Eres un asistente útil.")
};

var clientTransport = new StdioClientTransport(new()
{
    Name = "Servidor Calculadora",
    Command = "dotnet",
    Arguments = ["run", "--project", "../McpCalculatorServer/McpCalculatorServer.csproj"],
});
```

> 💡 Igual que en el Lab 2, puedes reemplazar `Command`/`Arguments` para apuntar directo al `.exe` ya compilado y que arranque más rápido:
> ```csharp
> Command = @"..\McpCalculatorServer\bin\Debug\net8.0\McpCalculatorServer.exe",
> Arguments = [],
> ```

```csharp
await using var mcpClient = await McpClient.CreateAsync(clientTransport);

ChatCompletionsToolDefinition ConvertirATool(string name, string description, JsonElement schema)
{
    var functionDefinition = new FunctionDefinition(name)
    {
        Description = description,
        Parameters = BinaryData.FromObjectAsJson(
            new { Type = "object", Properties = schema },
            new JsonSerializerOptions { PropertyNamingPolicy = JsonNamingPolicy.CamelCase })
    };
    return new ChatCompletionsToolDefinition(functionDefinition);
}

// 1. Listar y convertir las tools del servidor MCP
var mcpTools = await mcpClient.ListToolsAsync();
var toolDefinitions = new List<ChatCompletionsToolDefinition>();

foreach (var tool in mcpTools)
{
    tool.JsonSchema.TryGetProperty("properties", out var propiedades);
    toolDefinitions.Add(ConvertirATool(tool.Name, tool.Description, propiedades));
}

// 2. Prompt del usuario
var userMessage = "Suma 12 y 30";
chatHistory.Add(new ChatRequestUserMessage(userMessage));

var options = new ChatCompletionsOptions(chatHistory)
{
    Model = "gpt-4.1-mini",
};
foreach (var t in toolDefinitions) options.Tools.Add(t);

// 3. Llamar al LLM
ChatCompletions response = await chatClient.CompleteAsync(options);

// 4. Si el LLM pide invocar una tool, la ejecutamos en el servidor MCP
foreach (var call in response.Value.ToolCalls)
{
    Console.WriteLine($"El LLM solicitó: {call.Name} con argumentos {call.Arguments}");
    var args = JsonSerializer.Deserialize<Dictionary<string, object>>(call.Arguments);

    var result = await mcpClient.CallToolAsync(
        call.Name,
        args!,
        cancellationToken: CancellationToken.None);

    Console.WriteLine($"Resultado: {result.Content.OfType<TextContentBlock>().First().Text}");
}
```

**Paso 3 — Ejecutar**

```bash
dotnet run
```

---

### 🐍 Python

**Paso 1 — Instalar dependencias adicionales**

```bash
pip install azure-ai-inference azure-core
```

**Paso 2 — Escribir el cliente con LLM**

Crea `client_llm.py`:

```python
import os
import json
import asyncio
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client
from azure.ai.inference import ChatCompletionsClient
from azure.core.credentials import AzureKeyCredential


def convertir_a_tool_llm(tool):
    """Convierte una tool del servidor MCP al formato que entiende el LLM"""
    return {
        "type": "function",
        "function": {
            "name": tool.name,
            "description": tool.description,
            "parameters": {
                "type": "object",
                "properties": tool.inputSchema["properties"],
            },
        },
    }


def llamar_llm(prompt, tools):
    token = os.environ["GITHUB_TOKEN"]
    endpoint = "https://models.inference.ai.azure.com"
    model_name = "gpt-4o"

    client = ChatCompletionsClient(endpoint=endpoint, credential=AzureKeyCredential(token))

    response = client.complete(
        messages=[
            {"role": "system", "content": "Eres un asistente útil."},
            {"role": "user", "content": prompt},
        ],
        model=model_name,
        tools=tools,
        temperature=1.0,
        max_tokens=1000,
        top_p=1.0,
    )

    message = response.choices[0].message
    llamadas = []

    if message.tool_calls:
        for call in message.tool_calls:
            name = call.function.name
            args = json.loads(call.function.arguments)
            llamadas.append({"name": name, "args": args})

    return llamadas


server_params = StdioServerParameters(command="mcp", args=["run", "server.py"], env=None)


async def run():
    async with stdio_client(server_params) as (read, write):
        async with ClientSession(read, write) as session:
            await session.initialize()

            # 1. Listar y convertir tools del servidor
            tools_result = await session.list_tools()
            tools_llm = [convertir_a_tool_llm(t) for t in tools_result.tools]

            # 2. Prompt del usuario
            prompt = "Suma 12 y 30"
            llamadas = llamar_llm(prompt, tools_llm)

            # 3. Ejecutar en el servidor MCP las tools que el LLM solicitó
            for llamada in llamadas:
                resultado = await session.call_tool(llamada["name"], arguments=llamada["args"])
                print("Resultado:", resultado.content)


if __name__ == "__main__":
    asyncio.run(run())
```

**Paso 3 — Ejecutar**

```bash
python client_llm.py
```

---

### ✅ Puntos clave del Lab 3

- Añadir un LLM al cliente mejora radicalmente la experiencia: el usuario escribe lenguaje natural, no comandos exactos.
- Es indispensable convertir el esquema de las tools de MCP al formato que el LLM espera (`type: function`, `parameters`, etc.).
- El ciclo prompt → decisión del LLM → llamada MCP → resultado es el patrón base de cualquier agente que use MCP.

---

## 🛠️ Solución de problemas comunes

| Problema | Posible causa / solución |
|---|---|
| `Connection refused` al conectar el cliente | Verifica que el servidor esté corriendo y que la ruta/comando en el `transport` sea correcta |
| El Inspector no encuentra el servidor | Revisa que el comando y los argumentos configurados apunten al ejecutable o script correcto |
| Error de autenticación con GitHub Models | Verifica que `GITHUB_TOKEN` esté configurado y que el token tenga el permiso **Models** habilitado |
| `ModuleNotFoundError: mcp` (Python) | Confirma que activaste el entorno virtual antes de instalar/ejecutar (`source venv/bin/activate`) |
| Paquete `ModelContextProtocol` no encontrado (C#) | Instálalo con `dotnet add package ModelContextProtocol --version 1.4.1` |
| Error `"unrecognized_keys": ["resultType"]` al conectar el Inspector (C#) | Estás usando una versión *prerelease* del SDK que ya implementa cambios del protocolo que el Inspector aún no reconoce. Corrige con `dotnet remove package ModelContextProtocol` y `dotnet add package ModelContextProtocol --version 1.4.1` |
| `error CS1061: 'ContentBlock' does not contain a definition for 'Text'` (C#) | En la versión `1.4.1` del SDK, `Content` es una colección de tipos polimórficos. Usa `result.Content.OfType<TextContentBlock>().First().Text` en vez de `result.Content.First(c => c.Type == "text").Text`, y agrega `using ModelContextProtocol.Protocol;` |
| `Connection Error - Check if your MCP server is running and proxy token is correct` | Cierra todas las pestañas del Inspector, mata procesos de Node colgados (`taskkill /F /IM node.exe` en Windows) y vuelve a abrir la URL completa con el token que imprime la terminal (`http://localhost:6274/?MCP_PROXY_AUTH_TOKEN=...`) |
| `Connection Error - Did you add the proxy session token in Configuration?` | El navegador abrió el Inspector sin el token en la URL. Pega el valor que aparece después de `🔑 Session token:` en el campo **Proxy Session Token** dentro de la sección **Configuration** de la interfaz |
| El LLM no invoca ninguna tool | Revisa que el esquema de la tool esté bien formado (`properties`, `required`) y que el prompt sea explícito sobre la operación deseada |

---

## 📚 Recursos adicionales

- 📘 [Documentación oficial de MCP](https://modelcontextprotocol.io/)
- 📜 [Especificación de MCP](https://modelcontextprotocol.io/specification/2025-11-25)
- 🧑‍💻 [MCP C# SDK](https://github.com/modelcontextprotocol/csharp-sdk)
- 🧑‍💻 [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
- 🔍 [MCP Inspector](https://github.com/modelcontextprotocol/inspector)
- 📖 [Curso completo MCP for Beginners (Microsoft)](https://github.com/microsoft/mcp-for-beginners)

---

## 🙏 Créditos y licencia

Este taller es una adaptación con fines educativos del contenido del **Lab 1 (First Server)**, **Lab 2 (First Client)** y **Lab 3 (Client with LLM)** del curso [**MCP for Beginners**](https://github.com/microsoft/mcp-for-beginners) de Microsoft, reducida a los lenguajes **C#** y **Python**. El curso original está disponible bajo licencia **MIT** y cubre además Java, JavaScript, Rust y TypeScript, junto con módulos avanzados de seguridad, despliegue e integración con Azure.

Si este taller te resultó útil, considera dar una ⭐ al [repositorio original](https://github.com/microsoft/mcp-for-beginners).
