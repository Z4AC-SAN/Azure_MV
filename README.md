# AZURE_MV

Guía paso a paso para aprovisionar una máquina virtual (VM) en Microsoft Azure desde cero, incluyendo la configuración de la cuenta, grupo de recursos y red virtual (VNet).

---

## Prerrequisitos
- Una cuenta de Microsoft (Outlook, Hotmail o cuenta corporativa/educativa).
- Un método de pago válido (tarjeta de crédito/débito) o acceso a **Azure for Students** / créditos gratuitos.

---

## 1. Crear una Cuenta en Azure

1. Ve al portal oficial de [Azure](https://azure.microsoft.com/).
2. Haz clic en **Empiece gratis** (o accede a través del portal de estudiantes si aplica).
3. Inicia sesión con tu cuenta de Microsoft.
4. Completa el formulario de identidad:
   - País/región y datos personales.
   - Verificación de identidad por teléfono (SMS o llamada).
   - Verificación de tarjeta (no genera cargos inmediatos en el tier gratuito).
5. Acepta los términos y finaliza el registro.
6. Ingresa a la consola de administración en [Azure Portal](https://portal.azure.com/).

---

## 2. Crear un Grupo de Recursos (Resource Group)

El grupo de recursos funciona como un contenedor lógico para organizar los servicios asociados al proyecto.

1. En la barra de búsqueda superior de Azure Portal, escribe **Grupos de recursos** y selecciónalo.
2. Haz clic en **+ Crear** (o **+ Create**).
3. Configura los parámetros básicos:
   - **Suscripción:** Selecciona tu suscripción activa (ej. *Azure subscription 1* o *Azure for Students*).
   - **Grupo de recursos:** Asigna un nombre descriptivo (ej. `rg-produccion-01`).
   - **Región:** Selecciona la ubicación geográfica más cercana a ti o a tus usuarios (ej. `East US`, `South Central US`).
4. Haz clic en **Revisar y crear**.
5. Una vez validado, haz clic en **Crear**.

---

## 3. Crear una Red Virtual (Virtual Network - VNet)

La VNet proporciona aislamiento de red y define el espacio de direcciones IP privadas para tus recursos.

1. En la barra de búsqueda, escribe **Redes virtuales** y selecciónalo.
2. Haz clic en **+ Crear**.
3. En la pestaña **Aspectos básicos**:
   - **Suscripción:** Tu suscripción activa.
   - **Grupo de recursos:** Selecciona el grupo creado anteriormente (`rg-produccion-01`).
   - **Nombre de la red virtual:** Asigna un identificador (ej. `vnet-principal`).
   - **Región:** Debe coincidir con la región del Grupo de Recursos.
4. En la pestaña **Direcciones IP**:
   - **Espacio de direcciones IPv4:** Define el rango CIDR principal (ej. `10.0.0.0/16`).
   - Haz clic en **+ Agregar subred** (o edita la predeterminada `default`):
     - **Nombre de la subred:** `snet-servidores`
     - **Intervalo de direcciones de subred:** `10.0.1.0/24`
5. Haz clic en **Revisar y crear** y luego en **Crear**.

---

## 4. Crear la Máquina Virtual (Virtual Machine)

1. En la barra de búsqueda, escribe **Máquinas virtuales** y selecciona el servicio.
2. Haz clic en **+ Crear** > **Máquina virtual de Azure**.
3. Configura las pestañas de aprovisionamiento:

### Aspectos básicos
- **Suscripción:** Tu suscripción actual.
- **Grupo de recursos:** `rg-produccion-01`.
- **Nombre de la máquina virtual:** Asigna un nombre (ej. `vm-servidor-01`).
- **Región:** La misma que tu VNet (ej. `East US`).
- **Opciones de disponibilidad:** Selecciona *No se requiere redundancia de la infraestructura* (para entornos de prueba/desarrollo).
- **Tipo de seguridad:** Estándar.
- **Imagen:** Selecciona el sistema operativo (ej. `Ubuntu Server 24.04 LTS` o `Windows Server 2025 Datacenter`).
- **Arquitectura de máquina virtual:** `x64`.
- **Tamaño:** Elige el tamaño según tu cuota y necesidad (ej. `Standard_B1s` o `Standard_B2s` para costos bajos).

### Cuenta de administrador
- **Si seleccionaste Linux:**
  - **Tipo de autenticación:** Clave pública SSH (recomendado) o Contraseña.
  - **Nombre de usuario:** Asigna un nombre (ej. `azureuser`).
  - **Origen de clave pública SSH:** *Generar un nuevo par de claves* (descárgala cuando el portal lo solicite).
- **Si seleccionaste Windows:**
  - Define **Nombre de usuario** y **Contraseña** segura.

### Reglas de puertos de entrada
- **Puertos de entrada públicos:** *Permitir los puertos seleccionados*.
- **Seleccionar puertos de entrada:** 
  - Para Linux: `SSH (22)`
  - Para Windows: `RDP (3389)`

### Redes
- **Red virtual:** Selecciona la red creada previamente (`vnet-principal`).
- **Subred:** Verifica que apunte a `snet-servidores (10.0.1.0/24)`.
- **IP pública:** Deja la opción predeterminada para que Azure asigne una IP dinámica.
- **Grupo de seguridad de red (NSG):** Básico.

### Discos y Administración
- Configura el tipo de disco del sistema operativo (SSD estándar o HDD estándar para optimizar costos de laboratorio).
- Mantén las opciones predeterminadas de diagnóstico y apagado automático si deseas evitar consumo innecesario de créditos.

4. Haz clic en **Revisar y crear**.
5. Tras completarse la validación, haz clic en **Crear**. *(Si elegiste generar claves SSH, descárgalas inmediatamente cuando aparezca la ventana emergente).*

---

## 5. Conexión a la Máquina Virtual

Una vez finalizado el despliegue, haz clic en **Ir al recurso** para obtener la **Dirección IP pública**.

### Conexión por SSH (Linux)
Abre tu terminal y ejecuta:
```bash
chmod 400 ruta/a/tu/clave.pem
ssh -i ruta/a/tu/clave.pem azureuser@<TU_IP_PUBLICA>
