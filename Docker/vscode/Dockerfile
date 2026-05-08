FROM codercom/code-server

USER root
RUN apt-get update && apt-get install -y \
    python3 python3-pip python3-venv curl tar gzip \
    && apt-get clean

USER coder

# --- venv ---
RUN python3 -m venv /home/coder/nosql_venv

# --- instalacja wszystkiego naraz ---
RUN /home/coder/nosql_venv/bin/pip install --no-cache-dir \
    jupyterlab ipywidgets pyspark numpy pandas tqdm \
    tabulate delta-spark kafka-python jsonschema psycopg2-binary \
    confluent-kafka ipykernel

# --- KLUCZOWE: kernel do notebooków ---
RUN /home/coder/nosql_venv/bin/python -m ipykernel install \
    --user \
    --name=nosql_venv \
    --display-name "Python (nosql_venv)"

# --- ustaw venv jako default ---
ENV PATH="/home/coder/nosql_venv/bin:$PATH"

# --- vscode config ---
COPY --chown=coder:coder ./config.yaml /home/coder/.config/code-server/config.yaml
COPY --chown=coder:coder ./settings.json /home/coder/.local/share/code-server/User/settings.json

# --- extensions ---
RUN code-server --install-extension ms-python.python
RUN code-server --install-extension ms-toolsai.jupyter
