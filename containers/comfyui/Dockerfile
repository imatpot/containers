FROM python:3.10-slim

ARG UID=1000
ARG GID=1000
ENV UID=${UID}
ENV GID=${GID}

RUN getent group "${GID}" || groupadd -g -l "${GID}" comfyui && \
    useradd -l -u "${UID}" -g "${GID}" -m comfyui

COPY src /comfyui
RUN chown -R "${UID}:${GID}" /comfyui

RUN apt-get update && \
    apt-get install --no-install-recommends -y git ffmpeg && \
    rm -rf /var/lib/apt/lists/*

USER comfyui
WORKDIR /comfyui
SHELL ["/bin/bash", "-c"]

RUN echo "export PATH=\${PATH}:/home/comfyui/.local/bin" >> /home/comfyui/.profile && \
    source /home/comfyui/.profile && \
    pip install --no-cache-dir -r requirements.txt

EXPOSE 8188
CMD ["python", "main.py"]

