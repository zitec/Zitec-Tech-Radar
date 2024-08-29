FROM node:latest

ENV SCHEMA_FILE=""
ENV JSON_FILE=""

# Install ajv-cli
RUN npm install -g ajv-cli

# Set the working directory
WORKDIR /app

# Default command to run ajv-cli
CMD ["ajv", "validate", "-s", "$SCHEMA_FILE", "-d", "$JSON_FILE"]
