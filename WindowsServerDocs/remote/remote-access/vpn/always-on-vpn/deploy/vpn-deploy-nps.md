---
title: Instalar y configurar el servidor NPS
description: El procesamiento del servidor NPS de las solicitudes de conexión enviadas por el servidor VPN comprueba que el usuario tiene permiso para conectarse, la identidad del usuario y registra los aspectos de la solicitud de conexión que eligió al configurar cuentas RADIUS en NPS.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 08/30/2018
ms.openlocfilehash: d286f44e198aa13204b884da3fdf729f18b7553b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860458"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Paso 4. Instalación y configuración del servidor de directivas de redes (NPS)

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2 y Windows 10

- [**Siguiente:** Paso 3. Configurar el servidor de acceso remoto para Always On VPN](vpn-deploy-ras.md)
- [**Siguiente:** Paso 5. Configuración de DNS y firewall](vpn-deploy-dns-firewall.md)

En este paso, instalará el servidor de directivas de redes (NPS) para el procesamiento de las solicitudes de conexión enviadas por el servidor VPN:

- Realice la autorización para comprobar que el usuario tiene permiso para conectarse.
- Realización de la autenticación para comprobar la identidad del usuario.
- Realización de cuentas para registrar los aspectos de la solicitud de conexión que eligió al configurar cuentas de RADIUS en NPS.

Los pasos de esta sección le permiten completar los siguientes elementos:

1. En el equipo o la máquina virtual que planeó el servidor NPS y que se instalaron en la organización o en la red corporativa, puede instalar NPS.

   >[!TIP]
   >Si ya tiene uno o más servidores NPS en la red, no es necesario realizar la instalación del servidor NPS. en su lugar, puede usar este tema para actualizar la configuración de un servidor NPS existente.

> [!NOTE]
> No se puede instalar el servicio servidor de directivas de redes en Windows Server Core.

2. En el servidor NPS corporativo o de la organización, puede configurar NPS para que se ejecute como un servidor RADIUS que procesa las solicitudes de conexión recibidas desde el servidor VPN.

## <a name="install-network-policy-server"></a>Instalar el Servidor de directivas de redes

En este procedimiento, instalará NPS mediante Windows PowerShell o Administrador del servidor el Asistente para agregar roles y características. NPS es un servicio de rol del rol de servidor Servicios de acceso y directivas de redes.

>[!TIP]
>De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 en todos los adaptadores de red instalados. Al instalar NPS y habilitar Firewall de Windows con seguridad avanzada, se crean automáticamente las excepciones de Firewall para estos puertos para el tráfico IPv4 e IPv6. Si los servidores de acceso a la red están configurados para enviar tráfico RADIUS a través de puertos distintos de los predeterminados, quite las excepciones creadas en firewall de Windows con seguridad avanzada durante la instalación de NPS y cree excepciones para los puertos que se usan para Tráfico RADIUS.

**Procedimiento para Windows PowerShell:**

Para llevar a cabo este procedimiento con Windows PowerShell, ejecute Windows PowerShell como administrador y escriba el siguiente cmdlet:

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**Procedimiento para Administrador del servidor:**

1.  En Administrador del servidor, seleccione **administrar**y, a continuación, seleccione **Agregar roles y características**.
        Se abre el Asistente para agregar roles y características.

2.  En antes de empezar, seleccione **siguiente**.

    >[!NOTE] 
    >La página **antes de comenzar** del Asistente para agregar roles y características no se muestra si ya había seleccionado **omitir esta página de forma predeterminada** cuando se ejecutó el Asistente para agregar roles y características.

3.  En seleccionar tipo de instalación, asegúrese de que esté seleccionada la opción instalación basada en **características o en roles** y seleccione **siguiente**.

4.  En Seleccionar servidor de destino, asegúrese de que esté seleccionada la **opción seleccionar un servidor del grupo de servidores** .

5.  En grupo de servidores, asegúrese de que el equipo local está seleccionado y seleccione **siguiente**.

6.  En Seleccionar roles de servidor, en **roles**, seleccione **servicios de acceso y directivas de redes**. Se abre un cuadro de diálogo en el que se pregunta si debe agregar características requeridas para servicios de acceso y directivas de redes.

7.  Seleccione **Agregar características**y, a continuación, seleccione **siguiente** .

8.  En seleccionar características, seleccione **siguiente**y, en servicios de acceso y directivas de redes, revise la información proporcionada y luego seleccione **siguiente**.

9.  En seleccionar servicios de rol, seleccione **servidor de directivas de redes**.

10. Para las características necesarias para el servidor de directivas de redes, seleccione **Agregar características**y, a continuación, seleccione **siguiente**.

11. En confirmar selecciones de instalación, seleccione **reiniciar automáticamente el servidor de destino si es necesario**.

12. Seleccione **sí** para confirmar el seleccionado y, a continuación, seleccione **instalar**.
    
    En la página progreso de la instalación se muestra el estado durante el proceso de instalación. Una vez completado el proceso, se muestra el mensaje "instalación correcta en *NombreDeEquipo*", donde *NombreDeEquipo* es el nombre del equipo en el que instaló el servidor de directivas de redes.

13. Selecciona **Cerrar**.

## <a name="configure-nps"></a>Configuración de NPS

Después de instalar NPS, configure NPS para administrar todas las tareas de autenticación, autorización y contabilidad para la solicitud de conexión que recibe del servidor VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Registrar el servidor NPS en Active Directory

En este procedimiento, registrará el servidor en Active Directory para que tenga permiso de acceso a la información de la cuenta de usuario durante el procesamiento de solicitudes de conexión.

**Pasos**

1.  En Administrador del servidor, seleccione **herramientas**y, a continuación, seleccione **servidor de directivas de redes**. Se abre la consola NPS.

2.  En la consola de NPS, haga clic con el botón derecho en **NPS (local)** y, a continuación, seleccione **registrar servidor en Active Directory**.
   
     Se abrirá el cuadro de diálogo Servidor de directivas de redes.

3.  En el cuadro de diálogo servidor de directivas de redes, seleccione **Aceptar** dos veces.

Para obtener métodos alternativos para registrar NPS, consulte [registrar un servidor NPS en un dominio de Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurar las cuentas de servidor de directivas de redes

En este procedimiento, configure las cuentas del servidor de directivas de redes con uno de los siguientes tipos de registro:

- **Registro de eventos**. Se usa principalmente para la auditoría y la solución de problemas de intentos de conexión. Puede configurar el registro de eventos NPS mediante la obtención de las propiedades del servidor NPS en la consola NPS.

- **Registro de solicitudes de autenticación y cuentas de usuario en un archivo local**. Se usa principalmente para realizar el análisis de las conexiones y para la facturación. También se usa como herramienta de investigación de seguridad, ya que proporciona un método para realizar el seguimiento de la actividad de un usuario malintencionado después de un ataque. Puede configurar el registro de archivos local mediante el Asistente para configuración de cuentas.

- **Registro de solicitudes de autenticación y cuentas de usuario en una base de datos compatible con XML Microsoft SQL Server**. Se usa para permitir que varios servidores que ejecuten NPS tengan un origen de datos. Además, ofrece las ventajas de usar una base de datos relacional. Puede configurar el registro de SQL Server mediante el Asistente para configuración de cuentas.

Para configurar las cuentas de servidor de directivas de redes, consulte Configuración de las [cuentas de servidor de directivas de redes](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Agregar el servidor VPN como cliente RADIUS

En la sección [configurar el servidor de acceso remoto para VPN Always on](vpn-deploy-ras.md) , ha instalado y configurado el servidor VPN. Durante la configuración del servidor VPN, agregó un secreto compartido RADIUS en el servidor VPN.

En este procedimiento, usará la misma cadena de texto secreto compartido para configurar el servidor VPN como un cliente RADIUS en NPS. Use la misma cadena de texto que usó en el servidor VPN, o se producirá un error de comunicación entre el servidor NPS y el servidor VPN.

>[!IMPORTANT] 
>Al agregar un nuevo servidor de acceso a la red (servidor VPN, punto de acceso inalámbrico, conmutador de autenticación o servidor de acceso telefónico) a la red, debe agregar el servidor como cliente RADIUS en NPS para que NPS tenga en cuenta y pueda comunicarse con el servidor de acceso a la red.

**Pasos**

1. En el servidor NPS, en la consola NPS, haga doble clic en **clientes y servidores RADIUS**.

2. Haga clic con el botón secundario en **clientes RADIUS** y seleccione **nuevo**. Se abre el cuadro de diálogo nuevo cliente RADIUS.

3. Compruebe que la casilla **habilitar este cliente RADIUS** está activada.

4. En **nombre descriptivo**, escriba un nombre para mostrar para el servidor VPN.

5. En **dirección (IP o DNS)** , escriba la dirección IP o el FQDN de NAS.
     
     Si escribe el FQDN, seleccione **comprobar** si desea comprobar que el nombre es correcto y se asigna a una dirección IP válida.

6. En **secreto compartido**, haga lo siguiente:

    1. Asegúrese de que está seleccionado **manual** .

    2. Escriba la cadena de texto seguro que escribió también en el servidor VPN.

    3. Vuelva a escribir el secreto compartido en confirmar secreto compartido.

7. Seleccione **Aceptar**. El servidor VPN aparece en la lista de clientes RADIUS configurados en el servidor NPS.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configuración de NPS como RADIUS para conexiones VPN

En este procedimiento, configurará NPS como un servidor RADIUS en la red de la organización. En el NPS, debe definir una directiva que permita que solo los usuarios de un grupo específico tengan acceso a la organización o a la red corporativa a través del servidor VPN, y solo cuando utilicen un certificado de usuario válido en una solicitud de autenticación PEAP.

**Pasos**

1. En la consola de NPS, en configuración estándar, asegúrese de que está seleccionado **servidor RADIUS para conexiones de acceso telefónico o VPN** .

2. Seleccione **Configurar VPN o acceso telefónico**.
        
    Se abre el Asistente para configurar VPN o acceso telefónico.

3. Seleccione **conexiones de red privada virtual (VPN)** y seleccione **siguiente**.

4. En especificar el servidor de acceso telefónico o VPN, en clientes RADIUS, seleccione el nombre del servidor VPN que agregó en el paso anterior. Por ejemplo, si el nombre NetBIOS del servidor VPN es RAS1, seleccione **RAS1**.

5. Seleccione **Siguiente**.

6. En configurar métodos de autenticación, realice los pasos siguientes:

    1. Desactive la casilla **autenticación cifrada de Microsoft versión 2 (MS-CHAPv2)** .

    2. Active la casilla **Protocolo de autenticación extensible** para seleccionarla.

    3. En tipo (según el método de acceso y configuración de red), seleccione **Microsoft: EAP protegido (PEAP)** y, a continuación, seleccione **configurar**.
      
        Se abre el cuadro de diálogo Editar propiedades de EAP protegido.

    4. Seleccione **quitar** para quitar el tipo de EAP contraseña segura (EAP-MSCHAP V2).

    5. Seleccione **Agregar**. Se abre el cuadro de diálogo Agregar EAP.

    6. Seleccione **tarjeta inteligente u otro certificado**y, a continuación, haga clic en **Aceptar**.

    7. Seleccione **Aceptar** para cerrar editar propiedades de EAP protegido.

7. Seleccione **Siguiente**.

8. En especificar grupos de usuarios, siga estos pasos:

    1. Seleccione **Agregar**. Se abre el cuadro de diálogo Seleccionar Usuarios, Equipos, Cuentas de servicio o Grupos.

    2. Escriba **usuarios de VPN**y, luego, haga clic en **Aceptar**.

    3. Seleccione **Siguiente**.

9. En especificar filtros IP, seleccione **siguiente**.

10. En especificar configuración de cifrado, seleccione **siguiente**. No realice ningún cambio.

    Esta configuración solo se aplica a las conexiones de cifrado punto a punto (MPPE) de Microsoft, que no son compatibles con este escenario.

11. En especificar un nombre de dominio Kerberos, seleccione **siguiente**.

12. Seleccione **Finalizar** para cerrar el asistente.

## <a name="autoenroll-the-nps-server-certificate"></a>Inscripción automática del certificado de servidor NPS

En este procedimiento, actualizará directiva de grupo manualmente en el servidor NPS local. Cuando directiva de grupo se actualiza, si la inscripción automática de certificados está configurada y funciona correctamente, el equipo local se inscribe automáticamente en un certificado de la entidad de certificación (CA).

>[!NOTE]  
>Directiva de grupo actualiza automáticamente al reiniciar el equipo miembro del dominio o cuando un usuario inicia sesión en un equipo miembro del dominio. Además, directiva de grupo se actualiza periódicamente. De forma predeterminada, esta actualización periódica se produce cada 90 minutos con un desplazamiento aleatorio de hasta 30 minutos.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

**Pasos**

1. En NPS, abra Windows PowerShell.

2. En el símbolo del sistema de Windows PowerShell, escriba **gpupdate**y presione Entrar.

## <a name="next-steps"></a>Pasos siguientes

[Paso 5. Configuración de DNS y del firewall para Always On VPN](vpn-deploy-dns-firewall.md): en este paso, instalará el servidor de directivas de redes (NPS) mediante Windows PowerShell o el Asistente para agregar roles y características administrador del servidor. También puede configurar NPS para que controle todas las tareas de autenticación, autorización y contabilidad para las solicitudes de conexión que recibe desde el servidor VPN.