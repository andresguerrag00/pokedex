# ☁️ Creación de cuenta y configuración inicial en Microsoft Azure

Este documento explica **paso a paso cómo crear una cuenta en Azure** y los **recursos básicos necesarios** para trabajar en la nube: grupos de recursos, máquinas virtuales, redes y reglas de seguridad.  
Está pensado para usuarios que comienzan con Azure y desean configurar su entorno correctamente desde cero.

---

##  1. Crear una cuenta en Microsoft Azure

1. Visita [https://azure.microsoft.com](https://azure.microsoft.com)  
   y haz clic en **“Comenzar gratis”** o **“Start free”**.
2. Regístrate con un correo electrónico válido (puedes usar cuentas Microsoft, Gmail, etc.).
3. Ingresa tus datos personales y una forma de pago (solo para verificación, no se cobra inicialmente).
4. Microsoft ofrece **créditos gratuitos (USD 200 aprox.)** para nuevos usuarios, válidos durante 30 días.
5. Una vez creada la cuenta, accede al **Portal de Azure**:  
   👉 [https://portal.azure.com](https://portal.azure.com)

---

##  2. Familiarizarse con el Portal

El **Portal de Azure** es la consola gráfica principal para crear y administrar recursos.  
Desde el menú lateral puedes acceder a:
- **Resource Groups (Grupos de recursos)**  
- **Virtual Machines (Máquinas virtuales)**  
- **Networking (Redes, IPs, NSG)**  
- **Storage Accounts (Almacenamiento)**  
- **Cost Management + Billing (Costos y facturación)**

También puedes usar **Azure Cloud Shell**, una terminal integrada con CLI preinstalada, accesible desde el ícono “>_” en la parte superior derecha del portal.

---

##  3. Instalar la Azure CLI (opcional, recomendado)

Si prefieres trabajar desde la terminal, instala la **Azure CLI**:

- **Windows (PowerShell):**
  ```bash
  winget install Microsoft.AzureCLI
  ```
- **macOS (Homebrew):**
  ```bash
  brew install azure-cli
  ```
- **Linux (Ubuntu/Debian):**
  ```bash
  curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
  ```

Verifica la instalación:
```bash
az --version
```

Inicia sesión:
```bash
az login
```

---

##  4. Crear un grupo de recursos

Un **grupo de recursos** agrupa todos los servicios relacionados (VM, redes, discos, etc.).

Desde el portal:
1. Busca “Grupos de recursos”.
2. Haz clic en **Crear**.
3. Asigna un nombre (por ejemplo, `rg-pokedex`).
4. Selecciona una región (por ejemplo, `East US` o la más cercana a tu ubicación).
5. Haz clic en **Revisar y crear → Crear**.

O con CLI:
```bash
az group create --name rg-pokedex --location eastus
```

---


---

##  5. Configurar red y seguridad (NSG)

Cada VM tiene un **Network Security Group (NSG)** que controla los puertos abiertos.

Para abrir puertos adicionales desde la CLI:
```bash
az network nsg rule create   --resource-group rg-pokedex   --nsg-name vm-demoNSG   --name AllowHTTP   --priority 1001   --protocol Tcp   --destination-port-ranges 80   --access Allow   --direction Inbound
```

Repite el comando con los puertos `22`, `443`, `3000`, o los que necesites.

---

##  6. Conectarse por SSH

Después de crear la VM, obtén su IP pública:
```bash
az vm show --name vm-demo --resource-group rg-pokedex -d --query publicIps -o tsv
```

Luego conéctate:
```bash
ssh azureuser@<IP_PUBLICA>
```

Si estás en Windows, puedes usar PowerShell o Windows Terminal con SSH habilitado.

---

##  7. Asignar un nombre DNS (opcional)

Azure permite asociar un nombre DNS público a tu IP:
1. Abre tu VM → “Dirección IP pública”.
2. En la pestaña **Configuración**, busca el campo:
   ```
   DNS name label
   ```
3. Escribe un nombre único (por ejemplo, `vm-pokedex-demo`).
4. Guarda los cambios.

Tu dominio quedará así:
```
https://vm-pokedex-demo.eastus.cloudapp.azure.com
```

---

##  8. Supervisión y costos

- En el portal, abre **Cost Management + Billing** para revisar el consumo.
- Usa **Azure Advisor** para recomendaciones automáticas de ahorro y rendimiento.
- Detén la VM cuando no la uses:
  ```bash
  az vm deallocate -g rg-pokedex -n vm-demo
  ```

---

##  9. Buenas prácticas de seguridad

- Usa claves SSH en lugar de contraseñas.
- Cierra puertos no utilizados.
- Crea **alertas de costo** y **límites de gasto**.
- Actualiza el sistema operativo con frecuencia:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
- Usa roles y permisos específicos en lugar de cuentas con acceso total.

---

## Resumen

| Recurso | Descripción |
|----------|--------------|
| **Cuenta Azure** | Creada con plan gratuito o de pago |
| **Grupo de recursos** | `rg-pokedex` |
| **VM** | Ubuntu 22.04, 2 vCPU, 8 GB RAM |
| **Puertos abiertos** | 22, 80, 443 |
| **DNS opcional** | `*.cloudapp.azure.com` |
| **Herramienta CLI** | `az` instalada y configurada |

---

##  Recursos adicionales

- Portal de Azure: [https://portal.azure.com](https://portal.azure.com)
- CLI Reference: [https://learn.microsoft.com/cli/azure](https://learn.microsoft.com/cli/azure)
- Calculadora de precios: [https://azure.microsoft.com/pricing/calculator/](https://azure.microsoft.com/pricing/calculator/)
- Documentación oficial de máquinas virtuales: [https://learn.microsoft.com/azure/virtual-machines](https://learn.microsoft.com/azure/virtual-machines)

---

**Última actualización:** Octubre 2025  
**Propósito:** Guía general para crear y configurar recursos en Azure desde cero.
