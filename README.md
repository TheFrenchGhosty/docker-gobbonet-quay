# docker-gobbonet-quay

A [GobboNet](https://goblincorps.com/gobbonet) ([GitHub](https://github.com/ElodineOfficial/gobbonet)) image, on Quay.

[Quay page](https://quay.io/repository/thefrenchghosty/gobbonet)

## Usage

- Download (or copy the content of) the compose file that matches your setup:
  - `docker-compose.yml`- default, with a llama.cpp CPU sidecar.
  - `docker-compose.cuda.yml`- llama.cpp CUDA server (NVIDIA GPU).
  - `docker-compose.rocm.yml`- llama.cpp ROCm server (AMD GPU).
  - `docker-compose.vulkan.yml`- llama.cpp Vulkan server.
  - `docker-compose.no-llama.yml`- GobboNet only, pointing at an external llama.cpp server. Edit `GOBBONET_LLM_URL` in that file to match your server.
- Create the data directory and make it possible for the containers to write to it with their respecting users: `mkdir -p data && chmod -R 777 data`
- `docker compose up -d` (or `docker compose -f docker-compose.XYZ.yml up -d` for a GPU variant)
- The UI is available at `http://127.0.0.1:9066`.

Models are shared through `./data/models` - both GobboNet and llama.cpp mount the same `./data:/data` volume.


## Note

This specifically builds the Linux build from [jmccardle](https://github.com/jmccardle/gobbonet/tree/go_go_gobbonet) - that is officially endorsed by the GobboNet creators - it may or may not get merged with upstream (relevant PR: [here](https://github.com/ElodineOfficial/GobboNet/pull/2)).
