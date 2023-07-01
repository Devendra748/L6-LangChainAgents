# LangChain: Agents
LangChain is a Python library that provides a framework for building conversational agents using various tools and models. This README file explains how to use LangChain's built-in tools and how to define your own tools for creating powerful conversational agents.

# Installation
To use LangChain, you need to install it using pip:
```
pip install langchain
```
In addition, you need to install the required dependencies by running the following command:

```
pip install -r requirements.txt
```
# Built-in LangChain Tools
LangChain comes with several built-in tools that you can use to enhance your conversational agents. Some of these tools include DuckDuckGo search, Wikipedia, and Python REPL. To use these tools, you need to import them as follows:

```
from langchain.agents import load_tools, initialize_agent
from langchain.agents import AgentType
from langchain.tools.python.tool import PythonREPLTool
from langchain.chat_models import ChatOpenAI

llm = ChatOpenAI(temperature=0)
tools = load_tools(["llm-math", "wikipedia"], llm=llm)

agent = initialize_agent(
    tools,
    llm,
    agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION,
    handle_parsing_errors=True,
    verbose=True
)
```
Once you have initialized the agent, you can interact with it by passing a question or statement as input:

```
result = agent("What is the 25% of 300?")
```
You can also use the Python Agent, which provides a Python REPL interface. To create a Python agent, use the following code:

```
from langchain.agents.agent_toolkits import create_python_agent
from langchain.tools.python.tool import PythonREPLTool

agent = create_python_agent(
    llm,
    tool=PythonREPLTool(),
    verbose=True
)
```
The Python agent allows you to execute Python code and get the output directly in the conversation.

# Viewing Detailed Outputs of the Chains
If you want to view detailed outputs of the chains executed by the agent, you can enable the debug mode as follows:

```
import langchain
langchain.debug = True
```
After enabling the debug mode, you can run the agent and observe the detailed outputs:

```
agent.run(f"""Sort these customers by \
last name and then first name \
and print the output: {customer_list}""")
```
Remember to disable the debug mode when you no longer need the detailed outputs:

```
langchain.debug = False
```
# Defining Your Own Tool
LangChain allows you to define your own tools to extend the functionality of your conversational agents. To define a new tool, you need to create a Python function decorated with @tool from the langchain.agents module. Here's an example of defining a tool that returns today's date:

```
from langchain.agents import tool
from datetime import date

@tool
def time(text: str) -> str:
    """Returns today's date, use this for any \
    questions related to knowing today's date. \
    The input should always be an empty string, \
    and this function will always return today's \
    date - any date mathematics should occur \
    outside this function."""
    return str(date.today())
```
To use your custom tool, you can include it when initializing the agent:

```
agent = initialize_agent(
    tools + [time],
    llm,
    agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION,
    handle_parsing_errors=True,
    verbose=True
)
```
Now your agent can utilize the custom tool in its conversations.

# Note
Please note that the agents built with LangChain are still a work in progress, and they may occasionally come to the wrong conclusions. If you encounter any unexpected behavior, it is recommended to try running the agent again.

```
try:
    result = agent("What's the date today?")
except:
    print("Exception on external access")
```
# Conclusion
LangChain provides a flexible framework for building conversational agents using a variety of tools. By leveraging built-in tools and defining your own, you can create powerful agents capable of handling diverse conversational tasks. For more information and examples, please refer to the official documentation.
