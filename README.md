# Basic MCP Model with MCP server and MCP model


Math.py
```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("Math")

@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers together
    
    Args:
        a: The first number to add
        b: The second number to add
    
    Returns:
        The sum of the two numbers
    """
    return a + b

@mcp.tool()
def multiply(a: int, b: int) -> int:
    """Multiply two numbers together
    
    Args:
        a: The first number to multiply
        b: The second number to multiply
    
    Returns:
        The product of the two numbers
    """
    return a * b

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

weather.py file
```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("Weather")

@mcp.tool()
async def get_weather(city: str) -> str:
    """Get the weather for a given city
    
    Args:
        city: The city to get the weather for
    
    Returns:
        The weather for the given city
    """
    return f"The weather in {city} is sunny"


if __name__ == "__main__":
    mcp.run(transport="streamable-http")

```


client.py file 

- here we can integrate the different api servers with the help of langchain
``` python
from langchain_mcp_adapters.client import MultiServerMCPClient
from langgraph.prebuilt import create_react_agent
from langchain_groq import ChatGroq

from dotenv import load_dotenv
load_dotenv()
import asyncio
async def main():
    client = MultiServerMCPClient(
        {
            "math": {"command": "python", "args": ["mathserver.py"], "transport": "stdio"},
            "weather": {"url": "http://localhost:8001/mcp", "transport": "streamable_http"},
        }
    )

    import os 

    os.environ["GROQ_API_KEY"] = os.getenv("GROQ_API_KEY")

    tools = await client.get_tools()

    model= ChatGroq(model="qwen/qwen3-32b")

    agent = create_react_agent(model, tools)

    math_response = await agent.ainvoke({"input": "What is 2 + 2?"})
    print("math_response: ", math_response["messages"][-1].content)

    weather_response = await agent.ainvoke({"input": "What is the weather in Tokyo?"})
    print("weather_response: ", weather_response["messages"][-1].content)

asyncio.run(main())
```

- the math function in not imported tradintinoal from the mathserver
- Integrated her throuh the MultiServerMCPClient from langchain_mcp_adapters






    






 
