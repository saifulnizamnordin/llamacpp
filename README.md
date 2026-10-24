# llamacpp

llama.cpp server + built-in Web UI in Docker, running **Qwen3.6-35B-A3B** (35B MoE, ~3B active params) on CPU.

## Hardware this is tuned for

- Intel i7-11850H (8 cores), 64 GB RAM
- GPU: MX450 2 GB — too small to help, so inference runs on CPU

The MoE model only activates ~3B parameters per token, so it runs at usable speed on CPU,
unlike a dense 35B model.

## Usage

```powershell
docker compose up -d
docker compose logs -f     # first start downloads ~22 GB into ./models
```

Open the Web UI at <http://localhost:8080>.

An OpenAI-compatible API is served at `http://localhost:8080/v1`.

## Configuration

Copy `.env.example` to `.env` and edit:

| Variable   | Default                                      | Notes                         |
|------------|----------------------------------------------|-------------------------------|
| `PORT`     | `8080`                                       | Host port                     |
| `MODEL`    | `unsloth/Qwen3.6-35B-A3B-GGUF:UD-Q4_K_XL`    | Any HF `repo:quant` GGUF      |
| `CTX_SIZE` | `16384`                                      | Context length                |
| `THREADS`  | `8`                                          | Set to physical core count    |

Docker Desktop's WSL2 VM gets 50% of host RAM by default (~32 GB here), which fits this
22 GB model plus context. For larger quants or context, raise `memory=` in `%UserProfile%\.wslconfig`.

## Stop

```powershell
docker compose down
```
