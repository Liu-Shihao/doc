
# Step 1: Pick a function in your codebase that the model should be able to call

```python
# This is the function that we want the model to be able to call
def get_delivery_date(order_id: str) -> datetime:
    # Connect to the database
    conn = sqlite3.connect('ecommerce.db')
    cursor = conn.cursor()
    # ...
```

# Step 2: Describe your function to the model so it knows how to call it
```python
{
    "name": "get_delivery_date",
    "description": "Get the delivery date for a customer's order. Call this whenever you need to know the delivery date, for example when a customer asks 'Where is my package'",
    "parameters": {
        "type": "object",
        "properties": {
            "order_id": {
                "type": "string",
                "description": "The customer's order ID.",
            },
        },
        "required": ["order_id"],
        "additionalProperties": false,
    }
}
```

# Step 3: Pass your function definitions as available “tools” to the model, along with the messages

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_delivery_date",
            "description": "Get the delivery date for a customer's order. Call this whenever you need to know the delivery date, for example when a customer asks 'Where is my package'",
            "parameters": {
                "type": "object",
                "properties": {
                    "order_id": {
                        "type": "string",
                        "description": "The customer's order ID.",
                    },
                },
                "required": ["order_id"],
                "additionalProperties": False,
            },
        }
    }
]

messages = [
    {"role": "system", "content": "You are a helpful customer support assistant. Use the supplied tools to assist the user."},
    {"role": "user", "content": "Hi, can you tell me the delivery date for my order?"}
]

response = openai.chat.completions.create(
    model="gpt-4o",
    messages=messages,
    tools=tools,
)
```

# Step 4: Receive and handle the model response
If the model decides that no function should be called
If the model does not generate a function call, then the response will contain a direct reply to the user in the normal way that Chat Completions does.

For example, in this case chat_response.choices[0].message may contain:
```python
chat.completionsMessage(content='Hi there! I can help with that. Can you please provide your order ID?', role='assistant', function_call=None, tool_calls=None)
```

```python
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_delivery_date",
            "description": "Get the delivery date for a customer's order. Call this whenever you need to know the delivery date, for example when a customer asks 'Where is my package'",
            "parameters": {
                "type": "object",
                "properties": {
                    "order_id": {
                        "type": "string",
                        "description": "The customer's order ID."
                    }
                },
                "required": ["order_id"],
                "additionalProperties": False
            }
        }
    }
]

messages = []
messages.append({"role": "system", "content": "You are a helpful customer support assistant. Use the supplied tools to assist the user."})
messages.append({"role": "user", "content": "Hi, can you tell me the delivery date for my order?"})
// highlight-start
messages.append({"role": "assistant", "content": "Hi there! I can help with that. Can you please provide your order ID?"})
messages.append({"role": "user", "content": "i think it is order_12345"})
// highlight-end

response = client.chat.completions.create(
    model='gpt-4o',
    messages=messages,
    tools=tools
)
```
## If the model generated a function call

```python
Choice(
    finish_reason='tool_calls', 
    index=0, 
    logprobs=None, 
    message=chat.completionsMessage(
        content=None, 
        role='assistant', 
        function_call=None, 
        tool_calls=[
            chat.completionsMessageToolCall(
                id='call_62136354', 
                function=Function(
                    arguments='{"order_id":"order_12345"}', 
                    name='get_delivery_date'), 
                type='function')
        ])
)
```

## Handling the model response indicating that a function should be called


```python
# Extract the arguments for get_delivery_date
# Note this code assumes we have already determined that the model generated a function call. See below for a more production ready example that shows how to check if the model generated a function call
tool_call = response.choices[0].message.tool_calls[0]
arguments = json.loads(tool_call['function']['arguments'])

order_id = arguments.get('order_id')

# Call the get_delivery_date function with the extracted order_id
delivery_date = get_delivery_date(order_id)
```

# Step 5: Provide the function call result back to the model

```python
# Simulate the order_id and delivery_date
order_id = "order_12345"
delivery_date = datetime.now()

# Simulate the tool call response
response = {
    "choices": [
        {
            "message": {
                "role": "assistant",
                "tool_calls": [
                    {
                        "id": "call_62136354",
                        "type": "function",
                        "function": {
                            "arguments": "{'order_id': 'order_12345'}",
                            "name": "get_delivery_date"
                        }
                    }
                ]
            }
        }
    ]
}

# Create a message containing the result of the function call
function_call_result_message = {
    "role": "tool",
    "content": json.dumps({
        "order_id": order_id,
        "delivery_date": delivery_date.strftime('%Y-%m-%d %H:%M:%S')
    }),
    "tool_call_id": response['choices'][0]['message']['tool_calls'][0]['id']
}

# Prepare the chat completion call payload
completion_payload = {
    "model": "gpt-4o",
    "messages": [
        {"role": "system", "content": "You are a helpful customer support assistant. Use the supplied tools to assist the user."},
        {"role": "user", "content": "Hi, can you tell me the delivery date for my order?"},
        {"role": "assistant", "content": "Hi there! I can help with that. Can you please provide your order ID?"},
        {"role": "user", "content": "i think it is order_12345"},
        response['choices'][0]['message'],
        function_call_result_message
    ]
}

# Call the OpenAI API's chat completions endpoint to send the tool call result back to the model
response = openai.chat.completions.create(
    model=completion_payload["model"],
    messages=completion_payload["messages"]
)

# Print the response from the API. In this case it will typically contain a message such as "The delivery date for your order #12345 is xyz. Is there anything else I can help you with?"
print(response)
```

# Handling edge cases

```python
# Check if the conversation was too long for the context window
if response['choices'][0]['message']['finish_reason'] == "length":
    print("Error: The conversation was too long for the context window.")
    # Handle the error as needed, e.g., by truncating the conversation or asking for clarification
    handle_length_error(response)
    
# Check if the model's output included copyright material (or similar)
if response['choices'][0]['message']['finish_reason'] == "content_filter":
    print("Error: The content was filtered due to policy violations.")
    # Handle the error as needed, e.g., by modifying the request or notifying the user
    handle_content_filter_error(response)
    
# Check if the model has made a tool_call. This is the case either if the "finish_reason" is "tool_calls" or if the "finish_reason" is "stop" and our API request had forced a function call
if (response['choices'][0]['message']['finish_reason'] == "tool_calls" or 
    # This handles the edge case where if we forced the model to call one of our functions, the finish_reason will actually be "stop" instead of "tool_calls"
    (our_api_request_forced_a_tool_call and response['choices'][0]['message']['finish_reason'] == "stop")):
    # Handle tool call
    print("Model made a tool call.")
    # Your code to handle tool calls
    handle_tool_call(response)
    
# Else finish_reason is "stop", in which case the model was just responding directly to the user
elif response['choices'][0]['message']['finish_reason'] == "stop":
    # Handle the normal stop case
    print("Model responded directly to the user.")
    # Your code to handle normal responses
    handle_normal_response(response)
    
# Catch any other case, this is unexpected
else:
    print("Unexpected finish_reason:", response['choices'][0]['message']['finish_reason'])
    # Handle unexpected cases as needed
    handle_unexpected_case(response)
```
 
# Function calling with Structured Outputs

```shell
curl https://api.openai.com/v1/chat/completions/create \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-2024-08-06",
    "messages": [
      {
        "role": "system",
        "content": "You are a helpful customer support assistant. Use the supplied tools to assist the user."
      },
      {
        "role": "user",
        "content": "Hi, can you tell me the delivery date for my order #12345?"
      }
    ],
    "tools": [
      {
        "type": "function",
        "function": {
          "name": "get_delivery_date",
          "description": "Get the delivery date for a customer's order. Call this whenever you need to know the delivery date, for example when a customer asks 'Where is my package'",
          "parameters": {
            "type": "object",
            "properties": {
              "order_id": {
                "type": "string",
                "description": "The customer's order ID."
              }
            },
            "required": ["order_id"],
            "additionalProperties": false
          }
        },
        // highlight-start
        "strict": true
        // highlight-end
      }
    ]
  }'

```

# Configuring function calling behavior using the tool_choice parameter
By default, the model is configured to automatically select which functions to call, as determined by the `tool_choice: "auto"` setting.

We offer three ways to customize the default behavior:

1. To force the model to always call one or more functions, you can set `tool_choice: "required"`. The model will then always select one or more function(s) to call. This is useful for example if you want the model to pick between multiple actions to perform next.
2. To force the model to call a specific function, you can set `tool_choice: {"type": "function", "function": {"name": "my_function"}}`.
3. To disable function calling and force the model to only generate a user-facing message, you can either provide no tools, or set `tool_choice: "none"`.

Note that if you do either 1 or 2 (i.e. force the model to call a function) then the subsequent finish_reason will be "stop" instead of being "tool_calls".