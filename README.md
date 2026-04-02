Something

```
for port in 8000 8001 8002 8003; do
  echo "=== Port $port ==="
  curl -s "http://192.168.50.25:$port/openapi.json" 2>/dev/null | \
    jq -r '.paths | keys[]' 2>/dev/null || echo "No OpenAPI found"
done
```
