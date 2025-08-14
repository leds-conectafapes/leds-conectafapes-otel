FROM signoz/signoz-otel-collector:v0.111.38

COPY otel-collector-config.yaml /etc/otel-collector-config.yaml

CMD [ \
  "--config=/etc/otel-collector-config.yaml", \
  "--copy-path=/var/tmp/collector-config.yaml", \
  "--feature-gates=-pkg.translator.prometheus.NormalizeName" \
]
