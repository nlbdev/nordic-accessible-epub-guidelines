# Dockerfile to create a pandoc image with all of texlive

FROM texlive/texlive

# Add pandoc
RUN apt-get update && apt-get install -y \
    pandoc \
   && rm -rf /var/lib/apt/lists/*

WORKDIR /data

ENTRYPOINT ["/usr/bin/pandoc"]
