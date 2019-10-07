FROM python:alpine3.9

# Install bash
RUN apk add --no-cache bash

# Copy requirements
COPY ./requirements.txt /usr/src/requirements.txt

# Start in default source directory
WORKDIR /usr/src/

# Install Python modules
RUN pip install -r requirements.txt