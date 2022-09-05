FROM ghcr.io/runatlantis/atlantis:v0.19.7

RUN mkdir -p /var/lib/atlantis && chown atlantis:atlantis /var/lib/atlantis

CMD ["atlantis", "server", "--data-dir", "/var/lib/atlantis", "--repo-config", "/etc/atlantis/atlantis_config.yml"]
