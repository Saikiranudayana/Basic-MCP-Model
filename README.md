# Basic MCP Model with MCP server and MCP model


Math.py
```code
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
```code
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
