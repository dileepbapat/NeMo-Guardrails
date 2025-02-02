# Using LLMs hosted on NVIDIA AI Foundation

This guide teaches you how to use NeMo Guardrails with LLMs hosted on NVIDIA AI Foundation. It uses the [ABC Bot configuration](https://github.com/NVIDIA/NeMo-Guardrails/tree/develop/examples/bots/abc/README.md) and changes the model to `playground_mixtral_8x7b`.

## Prerequisites

Before you begin, ensure you have the following prerequisites in place:

1. Install the [langchain-nvidia-ai-endpoints](https://github.com/langchain-ai/langchain-nvidia/tree/main/libs/ai-endpoints) package:

```bash
pip install -U --quiet langchain-nvidia-ai-endpoints
```

2. An NVIDIA NGC account to access AI Foundation Models. To create a free account go to [NVIDIA NGC website](https://ngc.nvidia.com/).

3. An API key from NVIDIA AI Foundation Endpoints:
    -  Generate an API key by navigating to the AI Foundation Models section on the NVIDIA NGC website, selecting a model with an API endpoint, and generating an API key.
    -  Export the NVIDIA API key as an environment variable:

```bash
export NVIDIA_API_KEY=$NVIDIA_API_KEY # Replace with your own key
```

4. If you're running this inside a notebook, patch the AsyncIO loop.

```python
import nest_asyncio

nest_asyncio.apply()
```

## Configuration

To get started, copy the ABC bot configuration into a subdirectory called `config`:

```bash
cp -r ../../../../examples/bots/abc config
```

Update the `models` section of the `config.yml` file to the desired model supported by NVIDIA AI Foundation Endpoints:

```yaml
...
models:
  - type: main
    engine: nvidia_ai_endpoints
    model: playground_mixtral_8x7b
...
```

## Usage

Load the guardrails configuration:

```python
from nemoguardrails import LLMRails, RailsConfig

config = RailsConfig.from_path("./config")
rails = LLMRails(config)
```

Test that it works:

```python
response = rails.generate(messages=[
{
    "role": "user",
    "content": "How many vacation days do I have per year?"
}])
print(response['content'])
```

```
The ABC Company provides eligible employees with 20 days of paid vacation time per year, accrued monthly. However, I would need to access your specific information to provide an exact number of days you have taken or accrued. Please refer to the employee handbook or contact HR for more information.
```

You can see that the bot responds correctly.

## Conclusion

In this guide, you learned how to connect a NeMo Guardrails configuration to an NVIDIA AI Foundation LLM model. This guide uses `playground_mixtral_8x7b`, however, you can connect any other model by following the same steps.
