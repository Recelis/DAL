# Large Language Models

## Ollama

[docs](https://github.com/ollama/ollama?tab=readme-ov-file)

This is a tool that allows you to run LLMs locally.

## Quantized Models

Models tend to use 32 bit `floating point` numbers for its weights which is what `GPU` run on. However Ollama defaults to 8 bit quantized models which reduce the accuracy of the weights, but can be run on CPUs.
