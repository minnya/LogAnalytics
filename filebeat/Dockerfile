# filebeat/Dockerfile
FROM docker.elastic.co/beats/filebeat:7.17.11

# Switch to root user
USER root

RUN apt update -y && apt install -y nano

# Copy filebeat.yml and set ownership to root
COPY --chown=root:root ./filebeat.yml /usr/share/filebeat/filebeat.yml

# Set file permissions to be writable only by the owner
RUN chmod go-w /usr/share/filebeat/filebeat.yml

# Switch back to non-root user (if necessary)
USER filebeat

# Run Filebeat
CMD ["filebeat", "-e"]