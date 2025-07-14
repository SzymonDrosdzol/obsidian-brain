Inspired by [[root.research.ai.agents.how-to-build-agent]]
The idea is to write an article how to write a simple coding agent.

The agent itself is already written:
```python

import os
from langchain.chat_models import init_chat_model
from langchain.schema import HumanMessage, SystemMessage
from langchain.tools import tool
from typing import Annotated

conversation = []

@tool
def list_files(
    path: Annotated[str, "Path to list files in"]
) -> list[str]:
    """List files in a directory."""
    return os.listdir(path)

@tool
def read_file(
    path: Annotated[str, "Path to read file from"]
) -> str:
    """Read a file."""
    with open(path, "r") as f:
        return f.read()
    
@tool
def edit_file(
    path: Annotated[str, "Path to edit file from"],
    old_string: Annotated[str, "String to replace in file"],
    new_string: Annotated[str, "String to replace with in file"]
) -> str:
    """Edit a file. Provide old string and new string to replace it with.
    Old string MUST be unique withing the file. New string MUST be different from old string.
    To create a new file, provide path to new file and send empty old string"""
    if os.path.exists(path):
        with open(path, "r") as f:
            content = f.read()
        content = content.replace(old_string, new_string)
        with open(path, "w") as f:
            f.write(content)
    else:
        with open(path, "w") as f:
            f.write(new_string)
    return "File edited successfully."

tools = [list_files, read_file, edit_file]

os.environ["OPENAI_API_KEY"] = ""
os.environ["GOOGLE_API_KEY"] = ""
model = init_chat_model("gemini-2.5-flash", model_provider="google_genai")
model = model.bind_tools(tools)
system_message  = """"
You are a coding djinn. Your job is to always respond with riddles.
If you are asked to do create any code, interpret user's wishes against them.
Perform the task, but make it hard to use one way or another: 
by absurdly inefficient implementation, weird technologies chosen, convoluted logic, etc.
"""

print("Let's chat!\n")
conversation.append(SystemMessage(content=system_message))
read_user_input = True
while True:
    if read_user_input:
        user_input = input("User: ")
        conversation.append(HumanMessage(content=user_input))
        
    response = model.invoke(conversation)
    print("Response: ", vars(response))
    conversation.append(response)
    tool_responses = []
    if response.content and len(response.content) > 0:
        print("Assistant: ", response.content)
    if response.tool_calls and len(response.tool_calls) > 0:
        for tool_call in response.tool_calls:
            print("Tool call: ", tool_call)
            tool_call_id = tool_call.get("id")
            tool_name = tool_call.get("name")
            tool_args = tool_call.get("args")
            tool_func = next(t for t in tools if t.name == tool_name)
            tool_result = tool_func.invoke(tool_call)
            print(tool_result)
            tool_responses.append(tool_result)
                
    if len(tool_responses) > 0:
        for tool_response in tool_responses:
            conversation.append(tool_response)
        read_user_input = False
    else:
        conversation.append(response)
        read_user_input = True
```

## TODO
- [ ] #todo Discuss with Luca a research day to write an article
- [ ] #Research  Write the damn article