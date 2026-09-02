# AZURE_MV

Guía para aprovisionar una máquina virtual (VM) en Microsoft Azure, incluyendo la configuración de la cuenta, grupo de recursos y red virtual (VNet).

---

## 1. Crear una Cuenta en Azure

1. Ve al portal oficial de [Azure](https://azure.microsoft.com/).
2. O accede a través del portal de estudiantes.
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/bc8e5d4e-63c8-406c-8b90-050e8f79d05c" />
4. Inicia sesión con tu cuenta de Microsoft.
5. Completa el formulario de identidad:
   - País/región y datos personales.
   - Verificación de identidad por teléfono (SMS o llamada).
   - Verificación de tarjeta (no genera cargos inmediatos en el tier gratuito).
6. Acepta los términos y finaliza el registro.

---

## 2. Crear un Grupo de Recursos (Resource Group)

1. En la barra de búsqueda superior de Azure Portal, escribe **Resource group** y selecciónalo.
2. Haz clic en **+ Create**.
3. Configura los parámetros básicos:
   - **Suscription:** Selecciona tu suscripción activa (**Azure for Students**).
   - **Resource group:** Asigna un nombre descriptivo (`Teach`).
   - **Region:** Selecciona la ubicación geográfica (`Mexico Center`).
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/8378a43f-a434-40f9-b876-2bd81d1e2810" />
4. Haz clic en **review and create**.
5. Una vez validado, haz clic en **create**.
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/3ab244aa-f314-4fcf-bfbd-5aba5dc7a60e" />

---

## 3. Crear una Red Virtual (Virtual 

1. En la barra de búsqueda, escribe **virtual network** y selecciónalo.
2. Haz clic en **+ create**.
3. En la pestaña **basics**:
   - **Subscription:** Tu suscripción activa.
   - **Grupo de recursos:** Selecciona el grupo creado anteriormente (`Teach`).
   - **Nombre de la red virtual:** Asigna un identificador (`Vnet-Web`).
   - **Región:** Debe coincidir con la región del Grupo de Recursos.
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/c550f818-5557-41e4-af5c-9155224caeb9" />
4. En la pestaña **Direcciones IP**:
   - **Espacio de direcciones IPv4:** Define el rango principal ( `10.0.0.0/16`).
   - Haz clic en **+ Agregar subred** (o edita la predeterminada `default`):
     - **Nombre de la subred:** `Subnet-Web`
     - **Intervalo de direcciones de subred:** `10.0.1.0/24`
5. Haz clic en **Review and create** y luego en **Create**.
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/5c2c9191-f656-4fab-9f2e-b8a4b5bc103c" />

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
- **Size:** Elige el tamaño según tu cuota, necesidad y disponibilidad (`Standard_B1s` o `Standard_B2s` para costos bajos).
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/848fca0b-3dfe-490c-acad-9b2a7fd256fe" />

### Administrator account
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
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/29c7a779-9c83-46e7-b023-cbd098ad8b89" />

###Network security group
1. **create port rule:** agrega la opción de HTTP dejando todo por defecto
<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/0a73a5a0-924d-4a68-a580-325e218f0d12" />

---

## 5. Conexión a la Máquina Virtual

1. Una vez finalizado el despliegue, haz clic en **Conect** para obtener la **Dirección IP pública**(`ssh azureuser@158.23.184.28`).
2. Actualizar Ubuntu: `sudo apt update
sudo apt upgrade -y`

