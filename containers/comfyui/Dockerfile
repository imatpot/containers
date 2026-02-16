FROM python:3.13-slim AS builder

ARG UID=1000
ARG GID=1000

RUN apt-get update && \
    apt-get install --no-install-recommends -y git && \
    rm -rf /var/lib/apt/lists/*

RUN getent group "${GID}" || groupadd -g "${GID}" comfyui && \
    useradd -l -u "${UID}" -g "${GID}" -m comfyui

USER comfyui
WORKDIR /comfyui

COPY --chown="${UID}:${GID}" src/requirements.txt src/manager_requirements.txt* ./
RUN --mount=type=cache,target=/home/comfyui/.cache/pip,uid="${UID}",gid="${GID}" \
    pip install --user --no-cache-dir -r requirements.txt && \
    if [ -f manager_requirements.txt ]; then pip install --user --no-cache-dir -r manager_requirements.txt; fi

COPY --chown="${UID}:${GID}" src /comfyui
RUN python -m compileall -q /comfyui

FROM python:3.13-slim

ARG UID=1000
ARG GID=1000
ENV UID=${UID} \
    GID=${GID} \
    PATH="/home/comfyui/.local/bin:${PATH}" \
    PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    git \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN getent group "${GID}" || groupadd -g "${GID}" comfyui && \
    useradd -l -u "${UID}" -g "${GID}" -m comfyui

USER comfyui
WORKDIR /comfyui

COPY --from=builder --chown="${UID}:${GID}" /home/comfyui/.local /home/comfyui/.local
COPY --from=builder --chown="${UID}:${GID}" /comfyui /comfyui

EXPOSE 8188

CMD ["python", "main.py"]
