ARG CUDA_VERSION=11.8.0
FROM nvidia/cuda:${CUDA_VERSION}-cudnn8-runtime-ubuntu20.04
ARG TORCH_COMMAND

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -y && apt-get install -y --quiet --no-install-recommends \
	software-properties-common

RUN add-apt-repository -y ppa:deadsnakes/ppa
RUN apt-get update -y && apt-get install -y --quiet --no-install-recommends \
	gcc build-essential \
	libgl1 libglib2.0-0 \
	python3.10 python3.10-venv \
	python3.10-distutils pip \
	wget curl \
	git \
	vim \
	inetutils-ping \
	sudo \
	net-tools \
	iproute2 \
	google-perftools \
	&& \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 999 \
	&& update-alternatives --config python3 && ln -s /usr/bin/python3 /usr/bin/python

RUN curl -sS https://bootstrap.pypa.io/get-pip.py | python3.10

COPY webui.sh webui.sh

ENV install_dir=/
ENV TORCH_COMMAND="${TORCH_COMMAND}"
RUN ./webui.sh -f can_run_as_root --exit --skip-torch-cuda-test

ENV VIRTUAL_ENV=/stable-diffusion-webui/venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

WORKDIR "/stable-diffusion-webui/"
VOLUME /stable-diffusion-webui/models
VOLUME /root/.cache

CMD ["python3", "launch.py", "--listen"]
