FROM python:3.8-slim-buster
LABEL org.opencontainers.image.source https://github.com/flyteorg/flytesnacks

WORKDIR /root
ENV VENV /opt/venv
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PYTHONPATH /root

# This is necessary for opencv to work
RUN apt-get update && apt-get install -y libsm6 libxext6 libxrender-dev ffmpeg build-essential

# Install the AWS cli separately to prevent issues with boto being written over
RUN pip3 install awscli

ENV VENV /opt/venv
# Virtual environment
RUN python3 -m venv ${VENV}
ENV PATH="${VENV}/bin:$PATH"

# Install Python dependencies
COPY athena/requirements.txt /root
RUN pip install -r /root/requirements.txt

# Copy the makefile targets to expose on the container. This makes it easier to register
COPY in_container.mk /root/Makefile
COPY athena/sandbox.config /root

# Copy the actual code
COPY athena /root/athena

# This tag is supplied by the build script and will be used to determine the version
# when registering tasks, workflows, and launch plans
ARG tag
ENV FLYTE_INTERNAL_IMAGE $tag
