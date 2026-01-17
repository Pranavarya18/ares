> 🚀 ARES is under active development. More features and examples coming soon.
# ARES

ARES (Agentic Research and Evaluation Suite) is an RL-first framework for training and evaluating agents.

## Prerequisites

- Python 3.12 or higher
- [uv](https://docs.astral.sh/uv/) - Fast Python package installer and resolver

To install `uv`, follow the instructions at https://docs.astral.sh/uv/getting-started/installation/

## Installation

For now, we recommend running ARES locally from this directory:

`uv sync --all-groups`

and you're ready to get started.  
Alternatively, include it as a dependency in your own project's pyproject.toml using a relative path.  
PyPI installation will be coming soon.

## Quick Start (No API Keys)

You can quickly check if ARES is installed without setting up any API keys.

```bash
uv sync --all-groups
python -c "import ares; print('ARES installed successfully')"
```



## Configuration

ARES requires API keys for various services. To get started:

1. Copy the example environment file:  
   ```bash
   cp .env.example .env

Edit .env and fill in your API keys (see .env.example for required and optional variables)

## Full Setup (with API Keys)

ARES environments use an async version of the [dm_env](https://github.com/google-deepmind/dm_env) spec.  
Below is an example snippet of what this might look like in your code.

### Daytona Setup

By default, containers are run in Daytona:

1. Create a Daytona account at [https://www.daytona.io](https://www.daytona.io)  
2. Add your API key to `.env`:

By default, containers are run in Daytona, so you will need to:

Create a daytona account at https://www.daytona.io

Create a .env with DAYTONA_API_KEY=... and DAYTONA_API_URL=... set with an API key generated from your account.


### Martian Setup

For API inference:

1. Create an account at [https://app.withmartian.com](https://app.withmartian.com)  
2. Add your API key to `.env`:
This example also makes use of Martian for API inference. Similarly, you will need to:

Create an account at https://app.withmartian.com

Add CHAT_COMPLETION_API_KEY=... to your .env with a Martian API key.

Then, you can run the following example:

```python
import asyncio

from ares.code_agents import llms
from ares.environments import swebench_env

async def main():
    agent = llms.ChatCompletionCompatibleLLMClient(model="openai/gpt-4.1-mini")
    all_tasks = swebench_env.swebench_verified_tasks()
    tasks = [all_tasks[0]]  # Run on only one task for now.

    async with swebench_env.SweBenchEnv(tasks=tasks) as env:
        ts = await env.reset()
        while not ts.last():
            print(f"Observation: {ts.observation}")
            action = await agent(ts.observation)
            print(f"Action: {action}")
            ts = await env.step(action)

if __name__ == "__main__":
    asyncio.run(main())
## Status

- ✅ Local setup supported
- 🚧 PyPI release coming soon
- 🚧 More environments and agents in progress

