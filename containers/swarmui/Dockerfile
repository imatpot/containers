# adaptation of src/launchtools/StandardDockerfile.docker

FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim
ARG UID=1000

RUN apt-get update && \
    apt-get install --no-install-recommends -y git wget build-essential python3.11 python3.11-venv python3.11-dev python3-pip ffmpeg libglib2.0-0 libgl1 && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /SwarmUI
COPY src .

RUN chown -R "${UID}:${UID}" . && \
    useradd -l -u "${UID}" swarmui && \
    rm -rf ./src/bin ./src/obj

EXPOSE 7801
ENTRYPOINT ["bash", "/SwarmUI/launchtools/docker-standard-inner.sh"]
