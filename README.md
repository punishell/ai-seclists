Something

```
for port in 8000 8001 8002 8003; do
  echo "=== Port $port ==="
  curl -s "http://192.168.50.25:$port/openapi.json" 2>/dev/null | \
    jq -r '.paths | keys[]' 2>/dev/null || echo "No OpenAPI found"
done
```

Bash script that scans ports 8000–8010 on 192.168.50.25, queries each for an A2A Agent Card (/.well-known/agent.json), and prints any valid responses:

```
#!/bin/bash

TARGET_IP="192.168.50.25"
PORTS=(8000 8001 8002 8003)

for port in "${PORTS[@]}"; do
  echo "=== Port $port ==="
  response=$(curl -s --connect-timeout 2 "http://$TARGET_IP:$port/openapi.json" 2>/dev/null)
  
  if [[ -z "$response" ]]; then
    echo "No response or OpenAPI not found"
    continue
  fi

  # Try to parse the paths keys from the OpenAPI JSON
  paths=$(echo "$response" | jq -r '.paths | keys[]' 2>/dev/null)
  
  if [[ -z "$paths" ]]; then
    echo "No OpenAPI paths found or invalid JSON"
  else
    echo "$paths"
  fi
done

```
