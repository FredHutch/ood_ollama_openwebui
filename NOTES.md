docker run -d -p 3000:8080 -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama


mkdir ~/.ollama ~/.open-webui-data
apptainer run --bind $HOME/.ollama:/root/ollama,$HOME/.open-webui-data:/app/backend/data open-webui_ollama.sif bash


apptainer run  --bind ~/.ollama:/root/.ollama ollama_latest.sif

docker run --rm --name ollama -d -v /app/ollama:/root/.ollama ollama:latest


ml Apptainer

cd /hpc/temp/_ADM/SciComp/user/dtenenba
apptainer run --env OLLAMA_HOST=0.0.0.0:11444 --bind /app/ollama:/root/.ollama:ro /app/containers/ollama_latest.sif


apptainer run --env OLLAMA_HOST=0.0.0.0:11444 --bind /app/ollama:/root/.ollama:ro /hpc/temp/_ADM/SciComp/user/dtenenba/ollama_latest.sif


this works:

cd /hpc/temp/_ADM/SciComp/user/dtenenba/
apptainer run --env OLLAMA_HOST=0.0.0.0:11444  --bind ~/.ollama:/root/.ollama ollama_latest.sif

apptainer run --env OLLAMA_HOST=0.0.0.0:55555  --bind /app/ollama:/root/.ollama ollama_latest.sif bash


set -x DATA_DIR ~/.open-webui
set -x OLLAMA_BASE_URL http://127.0.0.1:11444
cd ~/dev/open-webui-frob
source .venv/bin/activate.fish
open-webui serve

curl http://localhost:11444/api/generate -d '{
  "model": "llama3",
  "prompt": "Hello, world"
}'

curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2",
  "prompt": "Hello, world"
}'



curl http://localhost:11444/api/generate -d '{
  "model": "llama3.2:1b",
  "prompt": "Hello, world"
}'

curl http://localhost:11444/api/generate -d '{
  "model": "llama3.2:latest",
  "prompt": "Hello, world"
}'



docker run -d -v ollama:/root/.ollama -p 11454:11434 --name ollama ollama/ollama

this is the sauce:

apptainer run --env OLLAMA_HOST=0.0.0.0:11444 --bind /app/ollama:/app/ollama:ro --env OLLAMA_MODELS=/app/ollama/models/ /hpc/temp/_ADM/SciComp/user/dtenenba/ollama_latest.sif

apptainer instance start --env OLLAMA_HOST=0.0.0.0:11444 --bind /app/ollama:/app/ollama:ro --env OLLAMA_MODELS=/app/ollama/models/ /app/containers/ollama_latest.sif ollama


apptainer run --env OLLAMA_HOST=127.0.0.1:11444 --bind /app/ollama:/app/ollama:ro --env OLLAMA_MODELS=/app/ollama/models/ /app/containers/ollama_latest.sif


apptainer instance list

apptainer instance list --logs



ollama_latest.sif is now in /app/containers

ollama image is based on ubuntu 20.04

docker run -p 65535:65535 -e OPENWEBUI_PORT=65535 -v /app/ollama:/root/.ollama ha

maybe I could do this with 2 separate apptainer images
using instances?

apptainer instance run does not work at all  and
apptainer instance start seems very broken
so we'll go old school and use nohup plus apptainer run ... 

like this, nohup not needed (sorta needed for the dev phase):

apptainer run --env OLLAMA_HOST=127.0.0.1:11444 --bind /app/ollama:/app/ollama:ro --env OLLAMA_MODELS=/app/ollama/models/ /app/containers/ollama_latest.sif > ollama.out 2>&1 &

# convert this to apptainer

apptainer run  --env PORT=2112 --bind $HOME/.open-webui:/app/backend/data  /app/containers/open-webui_main.sif

set -x OLLAMA_BASE_URL http://127.0.0.1:11444

apptainer run --env OLLAMA_BASE_URL=$OLLAMA_BASE_URL   /app/containers/open-webui_main.sif

apptainer run  --env PORT=2112 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main


apptainer run  --env PORT=2112 --bind $HOME/.open-webui:/app/backend/data  /app/containers/open-webui_main.sif /app/backend/start.sh

apptainer run  --env PORT=2112 --bind $HOME/.open-webui:/app/backend/data  /app/containers/open-webui_main.sif /app/backend/start.sh

### the openwebui image I made based on the Dockerfile here

/hpc/temp/_ADM/SciComp/user/dtenenba/open-webui.sif



apptainer run  --env OLLAMA_BASE_URL=http://localhost:11444 --env PORT=2112 --bind $HOME/.open-webui:/app/backend/data /hpc/temp/_ADM/SciComp/user/dtenenba/open-webui.sif

to run openwebui from the command line, not in a container:

OLLAMA_BASE_URL=http://localhost:11444 OLLAMA_MODELS=/app/ollama/models/ OPENWEBUI_PORT=65535 OLLAMA_HOST=localhost:11444 open-webui serve --port $OPENWEBUI_PORT


try this

https://discourse.openondemand.org/t/avoid-launching-a-web-browser/4060/14

