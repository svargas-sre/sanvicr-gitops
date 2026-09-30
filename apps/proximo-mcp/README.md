# Proximo MCP

Despliegue minimo de Proximo para acceder a Proxmox VE mediante MCP Streamable HTTP.

- Endpoint interno: `http://proximo-mcp.homelab-mcp.svc.cluster.local:41243/mcp`
- Endpoint local durante una prueba con port-forward: `http://127.0.0.1:41243/mcp`
- La imagen esta fijada a `ghcr.io/john-broadway/proximo:0.44.0`.
- El acceso dentro de LXC (`PROXIMO_ENABLE_EXEC`) y mediante QEMU Guest Agent (`PROXIMO_ENABLE_AGENT`) esta desactivado.
- No hay credenciales ni manifiestos de Secret en Git.

El Deployment espera un Secret llamado `proximo-mcp` en el namespace `homelab-mcp`, con estas claves:

| Clave | Contenido |
| --- | --- |
| `api-base-url` | `https://HOST:8006/api2/json` |
| `node` | Nombre del nodo predeterminado |
| `fingerprint` | Huella SHA-256 del certificado del nodo |
| `pve-token` | `USUARIO@REALM!TOKEN_ID=TOKEN_SECRET` |
| `mcp-bearer-token` | Token aleatorio utilizado por el cliente MCP |

El Secret se crea manualmente con `kubectl` desde un archivo temporal fuera de este repositorio. No se debe agregar el archivo con valores reales a Git.

Una vez desplegado, la prueba mas sencilla desde una computadora con acceso al API de Kubernetes sera:

```powershell
kubectl -n homelab-mcp port-forward service/proximo-mcp 41243:41243
```

El cliente MCP debera conectarse a `http://127.0.0.1:41243/mcp` y enviar `Authorization: Bearer <mcp-bearer-token>`.
