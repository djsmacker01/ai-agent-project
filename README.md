# 🤖 My AI Agent Project

I built this Python-based AI agent that uses OpenAI's GPT models to answer questions and perform calculations using custom tools. This project shows how I created an intelligent agent with memory, tool integration, and conversation capabilities.

## 🎯 What My AI Agent Does

My AI agent can:
- ✅ Answer general questions using GPT-4
- ✅ Perform mathematical calculations using a built-in calculator tool
- ✅ Maintain conversation memory across multiple interactions
- ✅ Handle multi-step reasoning problems
- ✅ Fallback gracefully between different GPT models

## 🚀 How to Run My Project

### What You'll Need

To run my AI agent, you'll need:
- Python 3.7+ installed
- An OpenAI API key
- Basic understanding of Python

### Step 1: Get My Code Running

```bash
# Clone my repository
git clone <your-repo-url>
cd ai-agent-project

# I recommend using a virtual environment
python -m venv venv

# Activate it
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install the packages I used
pip install openai python-dotenv
```

### Step 2: Set Up Your API Key

1. Create a `.env` file in the project root:
```bash
touch .env
```

2. Add your OpenAI API key to the `.env` file:
```env
OPENAI_API_KEY=your_api_key_here
```

> ⚠️ **Important**: I've already set up `.gitignore` to protect your API key - never commit the `.env` file!

### Step 3: Run My Agent

Open my Jupyter notebook and run the cells:

```bash
jupyter notebook AI_agent_project.ipynb
```

Or if you prefer running it directly:
```bash
python -c "exec(open('AI_agent_project.ipynb').read())"
```

## 📋 How I Built This Step-by-Step

### Stage 1: Setting Up the Foundation
```python
# Cell 1: Import libraries and load environment
import openai
import os
from dotenv import load_dotenv

load_dotenv()
api_key = os.getenv("OPENAI_API_KEY")
```

**What I did here:**
- I set up secure API key loading from the `.env` file
- Imported all the libraries I needed
- Made sure everything was working before moving forward

### Stage 2: Testing My API Connection
```python
# Cell 2: Test API connection
client = OpenAI(api_key=api_key)
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role":"user", "content": "Say 'API connection Successful'"}]
)
```

**My approach:**
- I tested my API key with a simple request first
- I built in fallback to GPT-3.5-turbo if GPT-4 isn't available
- This way I knew everything was working before building the agent

### Stage 3: Creating My Basic Agent
```python
# Cell 3: Create the Agent class
class Agent:
    def __init__(self, api_key):
        self.client = OpenAI(api_key=api_key)
        self.model = "gpt-4"
        self.system_message = "You are a helpful assistant..."
        self.messages = []  # Conversation memory
    
    def chat(self, message):
        # Process user input and return response
        # Maintains conversation history
```

**Key features I implemented:**
- **Memory**: I store conversation history in `self.messages`
- **System Instructions**: I defined how the agent should behave
- **Error Handling**: I made it gracefully fallback between models

### Stage 4: Adding Tool Integration
```python
# Cell 4: Calculator Tool
class CalculatorTool:
    def get_schema(self):
        # Defines the tool for OpenAI
        return {
            "type": "function",
            "function": {
                "name": "calculator",
                "description": "Performs mathematical calculations",
                "parameters": {...}
            }
        }
    
    def execute(self, expression):
        # Actually performs the calculation
        return {"result": eval(expression)}
```

**What I added:**
- **Tool Definition**: I told the AI what tools are available
- **Function Calling**: Now the AI can use tools to solve problems
- **Schema Format**: I used OpenAI's function calling format

### Stage 5: My Enhanced Agent with Tools
```python
# Cell 4: Enhanced Agent class
class Agent:
    def __init__(self, api_key, tools=None):
        self.tools = tools if tools else []
        self.tool_map = {tool.get_schema()["function"]["name"]: tool for tool in self.tools}
    
    def chat(self, message, max_iterations=10):
        # Handles tool calling and iteration
        while iteration < max_iterations:
            # AI decides whether to use tools
            # Executes tools if needed
            # Returns final answer
```

**Advanced features I built:**
- **Tool Integration**: The AI can decide when to use tools
- **Iteration Control**: I prevented infinite loops
- **Multi-step Reasoning**: It can break down complex problems

## 🧪 My Agent in Action

### Mathematical Calculations
```
User: "What is 120.09 * 623.09?"
My Agent: Uses calculator tool → "74826.8781"
```

### Multi-step Reasoning
```
User: "If my brother is 32 years younger than my mother, and my mother is 30 years older than me, and I am 20, how old is my brother?"
My Agent: 
1. Calculates mother's age: 20 + 30 = 50
2. Calculates brother's age: 50 - 32 = 18
3. Returns: "Your brother is 18 years old"
```

### General Knowledge
```
User: "What is the capital of France?"
My Agent: "The capital of France is Paris."
```

## 🔧 How You Can Customize My Agent

### Adding Your Own Tools

You can extend my agent by creating new tools. Here's the pattern I used:

```python
class WeatherTool:
    def get_schema(self):
        return {
            "type": "function",
            "function": {
                "name": "get_weather",
                "description": "Get current weather for a location",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "location": {
                            "type": "string",
                            "description": "City name"
                        }
                    },
                    "required": ["location"]
                }
            }
        }
    
    def execute(self, location):
        # Your weather API logic here
        return {"weather": "sunny", "temperature": "25°C"}
```

### Changing How My Agent Behaves

You can modify the system message to change my agent's personality:

```python
agent = Agent(api_key=api_key)
agent.system_message = "You are a coding assistant that helps with Python programming."
```

### Switching Between Models

I designed it to work with different models. You can change it like this:

```python
agent = Agent(api_key=api_key)
agent.model = "gpt-3.5-turbo"  # Faster, cheaper
# or
agent.model = "gpt-4o-mini"    # Balanced option
```

## 🛠️ Troubleshooting Issues I've Encountered

### Common Problems I've Solved

**1. API Key Error**
```
ValueError: OpenAI API key not found. check .env file
```
**How I fixed it**: Make sure your `.env` file exists and contains `OPENAI_API_KEY=your_key_here`

**2. Model Access Error**
```
GPT-4 not accessible: The model 'gpt-4' does not exist
```
**My solution**: I built in automatic fallback to GPT-3.5-turbo, or you can manually set the model

**3. Import Errors**
```
ModuleNotFoundError: No module named 'openai'
```
**Fix**: Run `pip install openai python-dotenv`

### When You Need Help

- Check your OpenAI API key permissions
- Verify your internet connection
- Make sure you have sufficient API credits
- Check the [OpenAI documentation](https://platform.openai.com/docs)

## 📚 Learning Resources

- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Function Calling Guide](https://platform.openai.com/docs/guides/function-calling)
- [Python Environment Management](https://docs.python.org/3/tutorial/venv.html)

## 🤝 Contributing to My Project

I'd love to see what you build! Feel free to:
- Add new tools to my agent
- Improve the error handling I implemented
- Add more example interactions
- Create additional agent personalities

## 📄 License

This project is open source and available under the MIT License.

---

**Thanks for checking out my AI agent! 🚀** 

*Built by me with ❤️ using OpenAI's GPT models and Python*
