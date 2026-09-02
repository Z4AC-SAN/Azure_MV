# AZURE_MV

Guía para aprovisionar una máquina virtual (VM) en Microsoft Azure, incluyendo la configuración de la cuenta, grupo de recursos y red virtual (VNet).

---

## 1. Crear una Cuenta en Azure

1. Ve al portal oficial de [Azure](https://azure.microsoft.com/).
2. Accede a través del portal de estudiantes.
3. Inicia sesión con tu cuenta de Microsoft.
4. Completa el formulario de identidad:
   - País/región y datos personales.
   - Verificación de identidad por teléfono (SMS o llamada).
   - Verificación de tarjeta (no genera cargos inmediatos en el tier gratuito).
5. Acepta los términos y finaliza el registro.

---

## 2. Crear un Grupo de Recursos (Resource Group)

1. En la barra de búsqueda superior de Azure Portal, escribe **Resource group** y selecciónalo.
2. Haz clic en **+ Create**.
3. Configura los parámetros básicos:
   - **Suscription:** Selecciona tu suscripción activa (**Azure for Students**).
   - **Resource group:** Asigna un nombre descriptivo (`Teach`).
   - **Region:** Selecciona la ubicación geográfica (`Mexico Center`).
4. Haz clic en **review and create**.
5. Una vez validado, haz clic en **create**.

---

## 3. Crear una Red Virtual (Virtual 

1. En la barra de búsqueda, escribe **virtual network** y selecciónalo.
2. Haz clic en **+ create**.
3. En la pestaña **basics**:
   - **Subscription:** Tu suscripción activa.
   - **Grupo de recursos:** Selecciona el grupo creado anteriormente (`Teach`).
   - **Nombre de la red virtual:** Asigna un identificador (`Vnet-Web`).
   - **Región:** Debe coincidir con la región del Grupo de Recursos.
4. En la pestaña **Direcciones IP**:
   - **Espacio de direcciones IPv4:** Define el rango principal ( `10.0.0.0/16`).
   - Haz clic en **+ Agregar subred** (o edita la predeterminada `default`):
     - **Nombre de la subred:** `Subnet-Web`
     - **Intervalo de direcciones de subred:** `10.0.1.0/24`
5. Haz clic en **Review and create** y luego en **Create**.

---

## 4. Crear la Máquina Virtual (Virtual Machine)

1. En la barra de búsqueda, escribe **Virtual Machine** y selecciona el servicio.
2. Haz clic en **+ Create** > **Azure Virtual Machine**.
3. Configura las pestañas de aprovisionamiento:

### Aspectos básicos
- **Suscription:** Tu suscripción actual.
- **Resource group:** `Teach`.
- **Virtual Machine Name:** Asigna un nombre (`VM-Web-02`).
- **Región:** La misma que tu VNet (`Mexico Center`).
- **Opciones de disponibilidad:** Selecciona *Infrastructure redundancy is not required* (para entornos de prueba/desarrollo).
- **Security:** Standard.
- **Image:** Selecciona el sistema operativo (`Ubuntu Server 24.04 LTS`).
- **Virtual machine architecture:** `x64`.
- **Size:** Elige el tamaño según tu cuota y necesidad (`Standard_B1s` o `Standard_B2s` para costos bajos).

### Cuenta de administrador
- **Linux:**
  - **Tipo de autenticación:** Contraseña.
  - **Nombre de usuario:** Asigna un nombre (`azureuser`).
  - Define **Nombre de usuario** y **Contraseña** segura.

### Reglas de puertos de entrada
- **Puertos de entrada públicos:** *Permitir los puertos seleccionados*.
- **Seleccionar puertos de entrada:** 
  - Linux: `SSH (22)`

4. Haz clic en **Review and Create**.
5. Tras completarse la validación, haz clic en **Crear**.*

---

## 5. Conexión a la Máquina Virtual

Una vez finalizado el despliegue, haz clic en **Ir al recurso** para obtener la **Dirección IP pública**.

### Conexión por SSH (Linux)
Abre tu terminal y ejecuta:
```bash
chmod 400 ruta/a/tu/clave.pem
ssh -i ruta/a/tu/clave.pem azureuser@<TU_IP_PUBLICA>
