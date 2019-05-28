---
title: Solución de problemas de conexiones de escritorio remoto
description: Solucionar problemas de procedimientos organizados por síntoma
ms.custom: na
ms.reviewer: rklemen; josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: RobVyK
manager: ''
ms.author: kaushika; rklemen; josh.bender; v-tea
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4bbdd17f5e6e2b161e0dda0e172ea862a9107841
ms.sourcegitcommit: 564158d760f902ced7f18e6d63a9daafa2a92bd4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2019
ms.locfileid: "64988337"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Solución de problemas de conexiones de escritorio remoto
Para obtener una explicación breve de algunos de los problemas más comunes de servicios de escritorio remoto (RDS), consulte [preguntas más frecuentes acerca de los clientes de escritorio remoto](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). En este artículo se describe varios enfoques más avanzados de solución de problemas de conexión. Muchos de estos procedimientos se aplican si está solucionando una configuración simple, como un equipo físico que se conecta a otro equipo físico o una configuración más complicada. Algunos procedimientos tratan los problemas que se producen sólo en escenarios más complicados de varios usuarios. Para obtener más información acerca de los componentes de escritorio remotos y cómo funcionan juntos, consulte [arquitectura de servicios de escritorio remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Muchos de los procedimientos que se describen en este artículo requieren tener acceso a varios equipos, algunos de los cuales es posible que deba acceder de forma remota. Para obtener más información sobre herramientas de administración remota y cómo configurarlas, consulte [remoto las herramientas de administración de servidor (RSAT) para los sistemas operativos de Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

En la lista siguiente, identifique el tipo de indicación de que están experimentando usted (o sus usuarios).

- [El cliente de escritorio remoto no se puede conectar con el escritorio remoto, pero no existen los síntomas específicos o mensajes (pasos para solucionar problemas generales)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [El cliente de escritorio remoto no se puede conectar con el escritorio remoto y recibe un mensaje "Clase no registrada"](#client-cannot-connect-class-not-registered)
- [El cliente de escritorio remoto no se puede conectar con el escritorio remoto y recibe un "no hay licencias disponibles" o el mensaje "error de seguridad"](#client-cannot-connect-no-licenses-available)
- [El usuario recibe un mensaje de "Acceso denegado", o debe proporcionar las credenciales dos veces](#user-cannot-authenticate-or-must-authenticate-twice)
- [Acerca de cómo conectarse, los recibe un mensaje "El servicio de escritorio remoto está ocupado actualmente"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [El cliente de escritorio remoto se desconecta y no se puede volver a conectarse a la misma sesión](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [El usuario se conecta a un equipo portátil remoto en una red inalámbrica y, a continuación, el equipo portátil se desconecta de la red](#remote-laptop-disconnects-from-wireless-network).
- [El usuario experimente un rendimiento deficiente o problemas con las aplicaciones remotas](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Para obtener una lista actual de códigos de desconexión RDP, consulte [ExtendedDisconnectReasonCode enumeración](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>No hay síntomas específicos o mensajes (pasos para solucionar problemas generales)

Siga estos pasos cuando un cliente de escritorio remoto no se puede conectar a un escritorio remoto, pero no proporciona los mensajes u otros síntomas que ayudaría a identifican la causa. Para resolver muchas de las causas más comunes de este tipo de problema, use los métodos siguientes:

- [Comprobar el estado del protocolo RDP](#check-the-status-of-the-rdp-protocol)
- [Comprobar el estado de los servicios RDP](#check-the-status-of-the-rdp-services)
- [Compruebe que está funcionando el agente de escucha RDP](#check-that-the-rdp-listener-is-functioning)
- [Compruebe el puerto de escucha RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Comprobar el estado del protocolo RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Comprobar el estado del protocolo RDP en un equipo local

Para comprobar y cambiar el estado del protocolo RDP en un equipo local, consulte [cómo habilitar Escritorio remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Si no están disponibles las opciones de escritorio remotas, consulte [comprobar si un objeto de directiva de grupo está bloqueando el RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Comprobar el estado del protocolo RDP en un equipo remoto

> [!IMPORTANT]  
> Siga los pasos descritos en esta sección con cuidado. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para comprobar y cambiar el estado del protocolo RDP en un equipo remoto, use una conexión de red del registro:

1. Seleccione **iniciar**, seleccione **ejecutar**y, a continuación, escriba **regedt32**.
2. En el Editor del registro, seleccione **archivo**y, a continuación, seleccione **conectar al registro de red**.
3. En el **Seleccionar equipo** diálogo cuadro, escriba el nombre del equipo remoto, seleccione **comprobar nombres**y, a continuación, seleccione **Aceptar**.
4. Vaya a **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\Control\\servidor de Terminal Server**.  
   ![Editor del registro, que muestra la entrada fDenyTSConnections](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - Si el valor de la **fDenyTSConnections** clave es **0**, a continuación, RDP está habilitado
   - Si el valor de la **fDenyTSConnections** clave es **1**, RDP está deshabilitada
5. Para habilitar RDP, cambie el valor de **fDenyTSConnections** desde **1** a **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Compruebe si un objeto de directiva de grupo (GPO) está bloqueando el RDP en un equipo local

Si no se puede activar el RDP en la interfaz de usuario o si el valor de **fDenyTSConnections** vuelve a **1** después de que la haya cambiado, un GPO puede estar reemplazando la configuración de nivel de equipo.

Para comprobar la configuración de directiva de grupo en un equipo local, abra una ventana del símbolo del sistema como administrador y escriba el siguiente comando:
```
gpresult /H c:\gpresult.html
```
Este comando finalice, abra gpresult.html. En **configuración del equipo\\plantillas administrativas\\componentes de Windows\\servicios de escritorio remoto\\Host de sesión de escritorio remoto\\conexiones**, buscar el **permiten a los usuarios conectarse de forma remota mediante el uso de servicios de escritorio remoto** directiva.

- Si el valor de esta directiva es **habilitado**, directiva de grupo no está bloqueando las conexiones RDP.
- Si el valor de esta directiva es **deshabilitado**, comprobar **GPO prevalente**. Éste es el GPO que está bloqueando las conexiones RDP.
![Un segmento de ejemplo de gpresult.html, en el que el GPO de dominio ** bloque RDP ** es deshabilitar RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Un segmento de ejemplo de gpresult.html, en el que ** Directiva de grupo Local ** es deshabilitar RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Compruebe si un GPO está bloqueando el RDP en un equipo remoto

Para comprobar la configuración de directiva de grupo en un equipo remoto, el comando es prácticamente el mismo que para un equipo local:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
El archivo que este comando genera (**gpresult -\<nombre_equipo\>.html**) usa el mismo formato de información como la versión del equipo local (**gpresult.html**) usa.

#### <a name="modifying-a-blocking-gpo"></a>Modificar un GPO bloqueo

Puede modificar esta configuración en el Editor de objetos de directiva de grupo (GPE) y la consola de administración de directiva de grupo (GPM). Para obtener más información sobre cómo usar la directiva de grupo, consulte [Advanced Group Policy Management](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Para modificar la directiva de bloqueo, use uno de los métodos siguientes:

- En GPE, tener acceso el nivel adecuado de GPO (por ejemplo, local o dominio) y vaya a **configuración del equipo\\plantillas administrativas\\componentes de Windows\\servicios de escritorio remoto\\ Host de sesión de escritorio remoto\\conexiones**\\**permiten a los usuarios conectarse de forma remota mediante el uso de servicios de escritorio remoto**.  
   1. Establecer la directiva en **habilitado** o **no configurado**.
   2. En los equipos afectados, abra una ventana del símbolo del sistema como administrador y ejecute el **gpupdate /force** comando.
- En GPM, vaya a la unidad organizativa en la que se aplica la directiva de bloqueo a los equipos afectados y eliminar la directiva de la unidad organizativa.

### <a name="check-the-status-of-the-rdp-services"></a>Comprobar el estado de los servicios RDP

En el equipo local (cliente) y el equipo remoto (destino), deben estar ejecutando los siguientes servicios:

- Servicios de escritorio remoto (TermService)
- Redirector de puerto de modo de usuario de servicios de escritorio remoto (UmRdpService)

Puede usar el complemento MMC de servicios para administrar los servicios de forma local o remota. También puede usar PowerShell de forma local o remota (si el equipo remoto está configurado para aceptar comandos de PowerShell remotos).

![Servicios de escritorio remoto en el complemento MMC de servicios. No modifique la configuración predeterminada del servicio.](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

En cualquiera de los equipos, si no se está ejecutando uno o ambos servicios, iniciarlos.

> [!NOTE]  
> Si inicia el servicio de servicios de escritorio remoto, haga clic en **Sí** reiniciar automáticamente el servicio de Redirector de puerto de UserMode de servicios de escritorio remoto.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Compruebe que está funcionando el agente de escucha RDP

> [!IMPORTANT]  
> Siga los pasos descritos en esta sección con cuidado. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

#### <a name="check-the-status-of-the-rdp-listener"></a>Comprobar el estado del agente de escucha RDP

Para este procedimiento, use una instancia de PowerShell que tenga permisos administrativos. Para un equipo local, también puede usar un símbolo del sistema que tenga permisos administrativos. Sin embargo, este procedimiento usa PowerShell, dado que los mismos comandos funcionan tanto local como remotamente.

1. Abra una ventana de PowerShell. Para conectarse a un equipo remoto, escriba **Enter-PSSession - ComputerName \<nombre_equipo\>** .
2. Enter **qwinsta**. 
    ![El comando qwinsta enumera los procesos que escuchan en los puertos del equipo.](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. Si la lista incluye **rdp-tcp** con el estado **escuchar**, el agente de escucha RDP está trabajando. Continúe con [comprobar el puerto de escucha RDP](#check-the-rdp-listener-port). De lo contrario, continúe en el paso 4.
4. Exportar la configuración de agente de escucha RDP desde un equipo de trabajo.
    1. Inicie sesión en un equipo que tenga la misma versión de sistema operativo que el equipo afectado y acceso del registro del equipo (por ejemplo, mediante el Editor del registro).
    2. Vaya a la entrada del registro siguiente:  
        **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\Control\\servidor de Terminal Server\\WinStations\\RDP-Tcp**
    3. Exportar la entrada a un archivo. reg. Por ejemplo, en el Editor del registro, haga clic en la entrada, seleccione **exportar**y, a continuación, escriba un nombre de archivo para la configuración exportada.
    4. Copie el archivo .reg exportado en el equipo afectado.
5. Para importar la configuración de agente de escucha RDP, abra una ventana de PowerShell que tenga permisos administrativos en el equipo afectado (o abra la ventana de PowerShell y conectarse de forma remota en el equipo afectado).
   1. Para realizar copias de seguridad de la entrada del registro existente, escriba el siguiente comando:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Para quitar la entrada del registro existente, escriba los siguientes comandos:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Para importar la nueva entrada del registro y, a continuación, reinicie el servicio, escriba los siguientes comandos:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      donde \<filename\> es el nombre del archivo .reg exportado.
6. Pruebe la configuración por intentar de nuevo la conexión a Escritorio remoto. Si sigue sin poder conectarse, reinicie el equipo afectado.
7. Si sigue sin poder conectarse, [comprobar el estado del certificado autofirmado RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Comprobar el estado del certificado autofirmado de RDP

1. Si sigue sin poder conectarse, abra el complemento MMC de certificados. Cuando le pida que seleccione el almacén de certificados para administrar, seleccione **cuenta de equipo**y, a continuación, seleccione el equipo afectado.
2. En el **certificados** carpeta bajo **escritorio remoto**, eliminar el certificado autofirmado de RDP. 
    ![Certificados de escritorio remotos en el complemento MMC certificados.](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. En el equipo afectado, reinicie el servicio de servicios de escritorio remoto.
4. Actualice el complemento certificados.
5. Si no se vuelven a crear el certificado autofirmado RDP [Compruebe los permisos de la carpeta MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Compruebe los permisos de la carpeta MachineKeys

1. En el equipo afectado, abra el explorador y, a continuación, vaya a **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Haga clic en **MachineKeys**, seleccione **propiedades**, seleccione **seguridad**y, a continuación, seleccione **avanzadas**.
3. Asegúrese de que están configurados los permisos siguientes:
      - Builtin\\administradores: Control total
      - Todos los usuarios: Lectura, escritura

### <a name="check-the-rdp-listener-port"></a>Compruebe el puerto de escucha RDP

En el equipo local (cliente) y el equipo remoto (destino), el agente de escucha RDP debe escuchar en el puerto 3389. Ninguna otra aplicación debe estar usando este puerto.

> [!IMPORTANT]  
> Siga los pasos descritos en esta sección con cuidado. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para comprobar o cambiar el puerto RDP, utilice el Editor del registro:

1. Seleccione Inicio, seleccione **ejecutar**y, a continuación, escriba **regedt32**.
      - Para conectarse a un equipo remoto, seleccione **archivo**y, a continuación, seleccione **conectar al registro de red**.
      - En el **Seleccionar equipo** diálogo cuadro, escriba el nombre del equipo remoto, seleccione **comprobar nombres**y, a continuación, seleccione **Aceptar**.
2. Abra el registro y vaya a **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\ \<escucha\>** . 
    ![La subclave del número de puerto para el protocolo RDP.](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. Si **PortNumber** tiene un valor distinto **3389**, cámbiela a **3389**. 
   > [!IMPORTANT]  
    > Puede trabajar con servicios de escritorio remoto usando otro puerto. Sin embargo, no se recomienda hacer esto. Solución de problemas de este tipo de configuración está fuera del ámbito de este artículo.
4. Después de cambiar el número de puerto, reinicie el servicio de servicios de escritorio remoto.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Compruebe que otra aplicación no está intentando usar el mismo puerto

Para este procedimiento, use una instancia de PowerShell que tenga permisos administrativos. Para un equipo local, también puede usar un símbolo del sistema que tenga permisos administrativos. Sin embargo, este procedimiento utiliza PowerShell porque los mismos comandos de trabajo tanto local como remotamente.

1. Abra una ventana de PowerShell. Para conectarse a un equipo remoto, escriba **Enter-PSSession - ComputerName \<nombre_equipo\>** .
2. Escriba el comando siguiente:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![El comando netstat genera una lista de puertos y los servicios que escuchan a ellos.](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. Busque una entrada para el puerto TCP 3389 (o el puerto RDP asignado) con el estado **Escuchando**. 
    > [!NOTE]  
   > El PID (identificador de proceso) del proceso o servicio que utiliza el puerto aparece en la columna PID.
1. Para determinar qué aplicación está utilizando el puerto 3389 (o el puerto RDP asignado), escriba el siguiente comando:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![El comando tasklist informa de los detalles de un proceso específico.](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. Busque una entrada para el número de PID que está asociado con el puerto (desde el **netstat** salida). Los servicios o procesos que están asociados con ese PID aparecen en la parte derecha.
1. Si una aplicación o servicio que no sean de servicios de escritorio remoto (TermServ.exe) usa el puerto, puede resolver el conflicto utilizando uno de los métodos siguientes:
      - Configurar la otra aplicación o servicio para utilizar un puerto diferente (recomendado).
      - Desinstalar la otra aplicación o servicio.
      - Configuración de RDP para utilizar un puerto diferente y, a continuación, reinicie el servicio de servicios de escritorio remoto (no recomendado).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Comprobar si un firewall está bloqueando el puerto RDP

Use la **psping** herramienta para comprobar si puede alcanzar el equipo afectado a través del puerto 3389.

1. En un equipo diferente desde el equipo afectado, descargue **psping** desde <https://live.sysinternals.com/psping.exe>.
2. Abra una ventana del símbolo del sistema como administrador, cambie al directorio donde instaló **psping**y, a continuación, escriba el siguiente comando:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Compruebe la salida de la **psping** para obtener los resultados como el siguiente comando:  
      - **Conectarse a \<equipo IP\>** : El equipo remoto es accesible.
      - **(0% perdidos)** : Todos los intentos de conexión se realizó correctamente.
      - **El equipo remoto rechazó la conexión de red**: El equipo remoto no está accesible.
      - **(100% perdidos)** : Error en todos los intentos de conexión.
1. Ejecute **psping** en varios equipos para probar su capacidad para conectarse con el equipo afectado.
1. Tenga en cuenta si el equipo afectado bloquea las conexiones desde todos los demás equipos, algunos otros equipos o solo a otro equipo.
1. Siguientes pasos recomendados:
      - Póngase en contacto los administradores de red para comprobar que la red permita el tráfico RDP en el equipo afectado.
      - Investigar las configuraciones de los firewalls entre los equipos de origen y el equipo afectado (incluido Windows Firewall en el equipo afectado) para determinar si un firewall está bloqueando el puerto RDP.

## <a name="client-cannot-connect-class-not-registered"></a>Cliente no se puede conectar "Clase no registrada"

Cuando intenta conectarse a un equipo remoto mediante el uso de un cliente que se está ejecutando Windows 10, versión 1709 o versiones posteriores, el cliente no puede conectar mientras el servidor Host de sesión de escritorio remoto informa de un mensaje que contiene el código de error "Clase no registrada (0 x 80040154)".

Este problema se produce si el usuario que está intentando conectarse tenga un perfil de usuario obligatorio. Para resolver este problema, instale el [24 de julio de 2018: KB4338817 (16299.579 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) actualizaciones de Windows 10.

## <a name="client-cannot-connect-no-licenses-available"></a>Cliente no puede conectarse, no hay licencias disponibles

Esta situación se aplica a las implementaciones que incluyen un servidor RDSH y un servidor de administración de licencias de escritorio remoto.

Identificar qué tipo de comportamiento que ven los usuarios:

- Se desconectó la sesión porque no hay licencias están disponibles o no está disponible ningún servidor de licencias
- Se denegó el acceso debido a un error de seguridad

Inicie sesión en el Host de sesión de escritorio remoto como un administrador de dominio y abra el diagnóstico de licencias de escritorio remoto. Busque mensajes como el siguiente:

  - El período de gracia para el servidor remoto ha expirado en servidor de Host de sesión de escritorio, pero no se configuró el servidor Host de sesión de escritorio remoto con los servidores de licencias. se denegarán las conexiones al servidor Host de sesión de escritorio remoto a menos que un servidor de licencias está configurado para el servidor Host de sesión de escritorio remoto.
  - Servidor de licencias \<nombre_equipo\> no está disponible. Esto puede deberse a problemas de conectividad de red, se detiene el servicio de administración de licencias de escritorio remoto en el servidor de licencias o licencias de escritorio remoto ya no está instalada en el equipo.

Estos problemas tienden a ser asociado con los siguientes mensajes de usuario:

  - La sesión remota se desconectó porque no hay ningún licencias de acceso de cliente de escritorio remoto disponibles para este equipo.
  - La sesión remota se desconectó porque no hay ningún servidores de licencias de escritorio remoto disponibles para proporcionar una licencia.

En este caso, [configurar el servicio de licencias de escritorio remoto](#configure-the-rd-licensing-service).

Si el diagnóstico de licencias de escritorio remoto se muestran otros problemas, como "el componente de protocolo RDP X.224 detectó un error en el flujo del protocolo y ha desconectado el cliente," puede ser un problema que afecta a los certificados de licencia. Estos problemas tienden a ser asociado con los mensajes de usuario, como las siguientes:

Debido a un error de seguridad, el cliente no pudo conectarse al Terminal server. Después de asegurarse de que ha iniciado sesión la red, intente volver a conectarse al servidor.

En este caso, [las claves del registro de certificado de actualización de la X509](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configurar el servicio de licencias de escritorio remoto

El siguiente procedimiento usa el administrador del servidor para realizar los cambios de configuración. Para obtener información acerca de cómo configurar y usar el administrador del servidor, consulte [administrador del servidor](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Abra el administrador del servidor y, a continuación, vaya a **servicios de escritorio remoto**.
2. En **Deployment Overview**, seleccione **tareas**y, a continuación, seleccione **editar las propiedades de implementación**.
3. Seleccione **licencias de escritorio remoto**y, a continuación, seleccione el modo de licencia adecuado para su implementación (**por dispositivo** o **por usuario**).
4. Escriba el nombre de dominio completo (FQDN) de su servidor de licencias de escritorio remoto y, a continuación, seleccione **agregar**.
5. Si tiene más de un servidor de licencias de escritorio remoto, repita el paso 4 para cada servidor. 
    ![Opciones de configuración del servidor de licencias de escritorio remoto en el administrador del servidor.](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Las claves del registro de certificado de actualización de la X509

> [!IMPORTANT]  
> Siga los pasos descritos en esta sección con cuidado. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para resolver este problema, copia de seguridad y, a continuación, las claves del registro de certificado de quitar el X509, reinicie el equipo y, a continuación, el servidor de licencias de reactivar el escritorio remoto. Para ello, siga estos pasos.

> [!NOTE]  
> Realice el procedimiento siguiente en cada uno de los servidores RDSH.

1. Abra el Editor del registro y vaya a **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. En el menú del registro, seleccione **Exportar archivo del registro**.
3. Tipo **certificado exportado** en el **nombre de archivo** cuadro y, a continuación, seleccione **guardar**.
4. Haga clic en cada uno de los siguientes valores, seleccionados **eliminar**y, a continuación, seleccione **Sí** para comprobar la eliminación:  
      - **Certificado**
      - **Certificado X509**
      - **Identificador de certificado X509**
      - **X509 Certificate2**
5. Salga del Editor del registro y, a continuación, reinicie el servidor RDSH.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>Usuario no se puede autenticar o debe autenticarse dos veces

Hay varios problemas que pueden causar problemas que afectan a la autenticación de usuario. Esta sección tratan los siguientes casos:

  - [Un usuario se deniega el acceso con un mensaje de "restricciones de tipo de inicio de sesión"](#access-denied-restricted-type-of-logon)
  - [Un usuario o aplicación se deniega el acceso con el evento 16965, "una llamada remota a la base de datos SAM se ha denegado"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un usuario no puede iniciar sesión con una tarjeta inteligente](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Si se bloquea el equipo remoto, un usuario debe escribir una contraseña dos veces](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un usuario no puede iniciar sesión y recibe mensajes de "Corrección de oracle de cifrado CredSSP" y "error de autenticación"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Después de actualizar los equipos cliente, algunos usuarios necesitan iniciar sesión dos veces](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Los usuarios se deniegan el acceso en una implementación que usa RemoteGuard con varios agentes de conexión a Escritorio remoto](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Acceso denegado, el tipo restringido de inicio de sesión

En esta situación, un usuario de Windows 10 que intentan conectarse a equipos de Windows 10 o Windows Server 2016 se deniega el acceso con el siguiente mensaje:

> Conexión a Escritorio remoto:  
> El administrador del sistema ha restringido el tipo de inicio de sesión (red o interactiva) que pueda usar. Para obtener ayuda, póngase en contacto con su administrador del sistema o el soporte técnico.

Este problema se produce cuando la autenticación de nivel de red (NLA) se requiere para las conexiones RDP, y el usuario no es un miembro de la **Remote Desktop Users** grupo. También puede producirse si el **Remote Desktop Users** grupo no se ha asignado a la **tener acceso a este equipo desde la red** derecho de usuario.

Para solucionar este problema, siga uno de estos pasos:

  - [Modificar asignación de derechos de usuario o la pertenencia a grupos del usuario](#modify-the-users-group-membership-or-user-rights-assignment).
  - Desactive NLA (no recomendado).
  - Usar a los clientes de escritorio remoto que no sean Windows 10. Por ejemplo, los clientes de Windows 7 no tiene este problema.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modificar asignación de derechos de usuario o la pertenencia a grupos del usuario

Si este problema afecta a un solo usuario, la solución más sencilla para este problema consiste en agregar el usuario a la **Remote Desktop Users** grupo.

Si el usuario ya es un miembro de este grupo (o si varios miembros del grupo tienen el mismo problema), compruebe la configuración de derechos de usuario en el equipo remoto de Windows 10 o Windows Server 2016.

1. Abra el Editor de objetos de directiva de grupo (GPE) y conéctese a la directiva local del equipo remoto.
2. Vaya a **configuración del equipo\\Windows configuración\\configuración de seguridad\\directivas locales\\asignación de derechos de usuario**, haga clic en **tener acceso a esta equipo desde la red**y, a continuación, seleccione **propiedades**.
3. Compruebe la lista de usuarios y grupos para **Remote Desktop Users** (o un grupo primario).
4. Si la lista no incluye **Remote Desktop Users** (o un elemento primario grupo como **todo el mundo**) necesita agregarlo a la lista (si tiene más de uno o dos equipos en su implementación, utilice un objeto de directiva de grupo) .  
    Por ejemplo, la pertenencia predeterminada para **tener acceso a este equipo desde la red** incluye **todo el mundo**. Si la implementación usa un objeto de directiva de grupo para quitar **todo el mundo**, es posible que deba restaurar el acceso al actualizar el objeto de directiva de grupo para agregar **Remote Desktop Users**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Se ha denegado el acceso denegado, una llamada remota a la base de datos de SAM

Este comportamiento es más probable que ocurra si los controladores de dominio ejecutan Windows Server 2016 o posterior, y los usuarios intentan conectarse mediante el uso de una aplicación de conexión personalizada. En concreto, las aplicaciones que tienen acceso a información de perfil del usuario en Active Directory se denegarán acceso.

Este comportamiento es consecuencia de un cambio a Windows. En Windows Server 2012 R2 y versiones anteriores de las versiones, cuando un usuario inicia sesión en un escritorio, contactos de administrador de conexión remota (RCM) remoto el controlador de dominio (DC) para consultar las configuraciones que son específicas de escritorio remoto en el objeto de usuario de dominio de Active Directory Services (AD DS). Esta información se muestra en la ficha perfil de servicios de escritorio remoto de propiedades del objeto de un usuario en el complemento MMC de los equipos y usuarios de Active Directory.

A partir de Windows Server 2016, RCM ya no consulta el objeto de los usuarios en AD DS. Si necesita RCM consultar AD DS porque están usando los atributos de servicios de escritorio remoto, debe habilitar manualmente el comportamiento anterior de RCM.

> [!IMPORTANT]  
> Siga los pasos descritos en esta sección con cuidado. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [copia de seguridad del registro para restaurarlo](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) en caso de que se producen problemas.

Para habilitar el comportamiento heredado de RCM en un servidor Host de sesión de escritorio remoto, configure las siguientes entradas del registro y, a continuación, reinicie el **servicios de escritorio remoto** servicio:  
  - **HKEY\_LOCAL\_máquina\\SOFTWARE\\directivas\\Microsoft\\Windows NT\\servicios de Terminal Server**
  - **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation nombre\>\\**  
      - Name: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valor: **1** (Decimal)

Para habilitar el comportamiento heredado de RCM en un servidor que no sea un servidor Host de sesión de escritorio remoto, configure estas entradas del registro y la siguiente entrada del registro adicionales (y, a continuación, reinicie el servicio):
  - **HKEY\_LOCAL\_máquina\\sistema\\CurrentControlSet\\Control\\servidor de Terminal Server**

Para obtener más información acerca de este comportamiento, vea KB 3200967 [cambia al administrador de conexión de remoto en Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Un usuario no puede iniciar sesión con una tarjeta inteligente

Este artículo aborda las tres circunstancias común en el que un usuario no puede iniciar sesión un equipo de escritorio remoto mediante el uso de una tarjeta inteligente:  
  - [El usuario no puede iniciar sesión una sucursal que utiliza un modo de sólo lectura controlador de dominio (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [El usuario no puede iniciar sesión un equipo de Windows Server 2008 SP2 se ha actualizado recientemente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [El usuario no puede mantener la sesión iniciado en un equipo de Windows que se ha actualizado recientemente, y el servicio de servicios de escritorio remoto deja de responder (se bloquea)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>No se puede iniciar sesión con una tarjeta inteligente en una sucursal con un RODC

Este problema se produce en las implementaciones que incluyen un servidor RDSH en un sitio de sucursal que usa un RODC. El servidor RDSH está hospedado en el dominio raíz. Los usuarios en el sitio de rama pertenecen a un dominio secundario y usan tarjetas inteligentes para la autenticación. El RODC está configurado en caché las contraseñas de usuario (el RODC pertenece a la **permite un grupo de replicación de contraseñas de RODC**). Cuando los usuarios intentan iniciar sesión en sesiones en el servidor RDSH, recibir mensajes, como "inicio de sesión intentado no es válido. Esto es debido a una información de autenticación o nombre de usuario incorrecta".

Se trata de un problema conocido en la forma en que la raíz del controlador de dominio y el RODC administran el cifrado de las credenciales de usuario. El controlador de dominio raíz usa una clave de cifrado para cifrar las credenciales y el RODC proporciona una clave de descifrado al cliente. Sin embargo, las dos claves no coinciden.

Para solucionar este problema, considere la posibilidad de cambiar la topología de controlador de dominio si se desactiva el almacenamiento en caché de contraseña en el RODC manualmente o mediante la implementación de un controlador de dominio grabable al sitio de rama. Un método alternativo es mover el servidor RDSH al mismo dominio secundario que los usuarios. Otra alternativa consiste en permitir a los usuarios iniciar sesión sin tarjetas inteligentes. Cualquiera de estas soluciones pueden requerir pone en peligro en el nivel de seguridad o rendimiento.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>No se puede iniciar sesión con una tarjeta inteligente para un equipo de Windows Server 2008 SP2

Este problema se produce cuando los usuarios inician sesión en un equipo de Windows Server 2008 SP2 se ha actualizado con KB4093227 (2018.4B). Cuando los usuarios intentan iniciar sesión con una tarjeta inteligente, se deniega el acceso con los mensajes como "No se encontraron certificados válidos. Compruebe que la tarjeta está insertada correctamente y se ajusta estrechamente." Al mismo tiempo, el equipo de Windows Server registra el evento de aplicación "se produjo un error al recuperar un certificado digital de la tarjeta inteligente insertada. Firma no válida."

Para resolver este problema, actualice el equipo de Windows Server con el 2018.06 B nuevo lanzamiento de 4093227 KB, [descripción de la actualización de seguridad para el protocolo de escritorio remoto (RDP) de Windows vulnerabilidad de denegación de servicio en Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>No se puede mantener la sesión iniciada sesión con una tarjeta inteligente y bloqueos del servicio de servicios de escritorio remoto

Este problema se produce cuando los usuarios iniciar sesión un equipo Windows o Windows Server que se ha actualizado con 4056446 KB. En primer lugar, es posible que pueda iniciar sesión el sistema mediante el uso de una tarjeta inteligente al usuario, pero, a continuación, recibe un "SCARTAR\_E\_n\_servicio" mensaje de error. El equipo remoto puede dejar de responder.

Para solucionar este problema, reinicie el equipo remoto.

Para resolver este problema, actualice el equipo remoto con la corrección adecuada:

  - Windows Server 2008 SP2: KB 4090928, [Windows pérdidas de identificadores en el proceso lsm.exe y pueden mostrar aplicaciones de tarjeta inteligente "SCARTAR\_E\_n\_servicio" errores](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [de 17 de mayo de 2018: KB4103724 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 y Windows 10, versión 1607: KB 4103720, [de 17 de mayo de 2018: KB4103720 (compilación del sistema operativo 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Si se bloquea el equipo remoto, un usuario debe escribir una contraseña dos veces

Este problema puede producirse cuando un usuario intenta conectarse a un escritorio remoto que ejecuta Windows 10 versión 1709 en una implementación en el que las conexiones RDP no requieren NLA. En estas condiciones, si se ha bloqueado el escritorio remoto, el usuario debe escribir sus credenciales dos veces al conectarse.

Para resolver este problema, actualice el equipo de Windows 10 versión 1709 con KB 4343893, [30 de agosto de 2018: KB4343893 (16299.637 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Usuario no puede iniciar sesión y recibe mensajes de "Corrección de oracle de cifrado CredSSP" y "error de autenticación"

Cuando los usuarios intentan iniciar sesión con cualquier versión de Windows de Windows Vista SP2 y versiones posteriores o Windows Server 2008 SP2 y versiones posteriores, que se deniegan el acceso y recepción mensajes, como la siguiente:

  - Se ha producido un error de autenticación. No se admite la función solicitada.
  - Esto puede deberse a actualizaciones de oracle cifrado CredSSP

"Corrección de oracle de cifrado CredSSP" hace referencia a un conjunto de actualizaciones de seguridad que se lanzó en marzo, abril y mayo de 2018. CredSSP es un proveedor de autenticación que procesa las solicitudes de autenticación para otras aplicaciones. El 13 de marzo de 2018, "3B" y las actualizaciones posteriores solucionar una vulnerabilidad de seguridad en el que un atacante podría transmitir credenciales de usuario para ejecutar código en el sistema de destino.

Las actualizaciones iniciales se ha agregado compatibilidad con un nuevo objeto de directiva de grupo, **cifrado Oracle corrección**, que tiene los siguientes valores posibles:

  - **Vulnerable**. Las aplicaciones cliente que usa CredSSP pueden revertir a las versiones no seguras, pero este comportamiento expone los escritorios remotos a los ataques. Servicios que usan CredSSP aceptan a los clientes que no se han actualizado.
  - **Mitigar**. Las aplicaciones cliente que usa CredSSP no pueden revertir a las versiones no seguras, pero servicios que usan CredSSP aceptan a los clientes que no se han actualizado.
  - **Force actualiza los clientes**. Las aplicaciones cliente que usa CredSSP no pueden revertir a versiones inseguras y servicios que utilizan CredSSP no aceptará a los clientes sin revisiones. 
    Nota: No se debe implementar esta configuración hasta que todos los hosts remotos compatible con la versión más reciente.

La actualización del 8 de mayo de 2018, puede cambiar el valor predeterminado **cifrado Oracle corrección** de **Vulnerable** a **mitigado**. Con este cambio en su lugar, los clientes de escritorio remoto que tienen las actualizaciones no se pueden conectar a servidores que no los tienen (o actualizan los servidores que no se han reiniciado). Para obtener más información sobre los efectos de las actualizaciones y los tipos de comunicación que bloquea, vea KB 4093492, [CredSSP se actualiza para CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver este problema, asegúrese de que todos los sistemas están completamente actualizados y reiniciar. Para obtener una lista completa de las actualizaciones y más información acerca de las vulnerabilidades, consulte [CVE-2018-0886 | Vulnerabilidad de ejecución de código remoto CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Para solucionar este problema hasta que finalizan las actualizaciones, consulte 4093492 KB para los tipos permitidos de conexiones. Si no hay ninguna alternativa viable es posible que considere la posibilidad de uno de los métodos siguientes:

  - Para los equipos cliente afectados, establezca el **cifrado Oracle corrección** directiva de copia en **Vulnerable**.
  - Modificar las siguientes directivas en el **configuración del equipo\\plantillas administrativas\\componentes de Windows\\servicios de escritorio remoto\\Host de sesión de escritorio remoto\\ Seguridad** carpeta Directiva de grupo:  
      - **Requieren el uso del nivel de seguridad específicas para las conexiones remotas (RDP)** : establecido en **habilitado** y seleccione **RDP**.
      - **Requiere autenticación de usuario para las conexiones remotas mediante autenticación a nivel de red**: establecido en **deshabilitado**.
      > [!IMPORTANT]  
      > Estas modificaciones reducen la seguridad de la implementación. Solo deben ser temporales, si se usa en absoluto.

Para obtener más información sobre cómo trabajar con directiva de grupo, consulte [modificar un GPO bloqueo](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Después de actualizar los equipos cliente, algunos usuarios necesitan iniciar sesión dos veces

Los usuarios en alguna versión de Windows 7 o Windows 10 1709 inicie sesión en una sesión de escritorio remoto y ver de inmediato una segunda pantalla de inicio de sesión. Este problema se produce si los equipos cliente han recibido las siguientes actualizaciones:

  - Windows 7: KB 4103718, [8 de mayo de 2018: KB4103718 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de mayo de 2018: KB4103727 (compilación del sistema operativo 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Para resolver este problema, asegúrese de que los equipos que desean que los usuarios conectarse (así como los servidores RDSH o RDVI) se actualizan por completo a través de junio de 2018. Esto incluye las siguientes actualizaciones:

  - Windows Server 2016: KB 4284880, [12 de junio de 2018: KB4284880 (compilación del sistema operativo 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junio de 2018: KB4284815 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junio de 2018: KB4284855 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junio de 2018: KB4284826 (paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [descripción de la actualización de seguridad para la vulnerabilidad de ejecución remota de código de CredSSP de Windows Server 2008, Windows Embedded POSReady 2009 y 2009 estándar de Windows Embedded: 13 de marzo de 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Los usuarios se deniegan el acceso en una implementación que usa a de Credential Guard remoto con varios agentes de conexión a Escritorio remoto

Este problema se produce en las implementaciones de alta disponibilidad que utilice a dos o más remoto escritorio agentes de conexión, si de Credential Guard remoto de Windows Defender está en uso. Los usuarios no se pueden iniciar sesión en equipos de escritorio remotos.

Este problema se produce porque usa Kerberos para la autenticación de Credential Guard remoto y restringe NTLM. Sin embargo, en una configuración de alta disponibilidad con equilibrio de carga, los agentes de conexión a Escritorio remoto no se admiten las operaciones de Kerberos.

Si necesita usar una configuración de alta disponibilidad con agentes de conexión a Escritorio remoto con equilibrio de carga, puede solucionar este problema mediante la deshabilitación de Credential Guard remoto. Para obtener más información sobre cómo administrar Windows Defender remota Credential Guard, consulte [credenciales proteger escritorio remoto con Windows Defender remota Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Sobre la conexión, usuario recibe el mensaje "El servicio de escritorio remoto está ocupado actualmente"

Para determinar una respuesta adecuada para este problema, siga estos pasos:

1. Los servicios de escritorio remoto no responde (por ejemplo, el cliente de escritorio remoto aparece "responder" en la pantalla de bienvenida).  
      - Si el servicio no responde, consulte [problema de memoria de servidor RDSH](#rdsh-server-memory-issue).
      - Si el cliente parece estar interactuando con el servicio con normalidad, continúe con el paso siguiente.
2. ¿Si uno o varios usuarios desconectan las sesiones de escritorio remoto, los usuarios pueden conectar a intentarlo?  
   - Si el servicio sigue denegar las conexiones independientemente de cuántos usuarios desconectar las sesiones, vea [problema del agente de escucha de escritorio remoto](#rd-listener-issue).
   - Si el servicio comienza a aceptar conexiones de nuevo después de un número de usuarios disponen de sus sesiones desconectadas, [comprobación de la directiva de límite de conexión](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problema de memoria de servidor RDSH

Se ha encontrado una fuga de memoria en algunos servidores de Windows Server 2012 R2 RDSH. Con el tiempo, estos servidores comenzar rechacen las conexiones a Escritorio remoto y los inicios de sesión de consola local con los mensajes como los siguientes:

> La tarea que está intentando no se puede completar porque el servicio de escritorio remoto está ocupado actualmente. Vuelva a intentarlo en unos minutos. Otros usuarios aún podrá iniciar sesión.

Los clientes de escritorio remoto intenta conectarse también dejan de responder.

Para solucionar este problema, reinicie el servidor RDSH.

Para resolver este problema, aplique 4093114 KB, [10 de abril de 2018: KB4093114 (paquete acumulativo mensual)] (file:///C:\\usuarios\\v jesits\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\% de abril de 2010, % 202018: KB4093114% 20\(mensual 20Rollup %\)), a los servidores RDSH.

### <a name="rd-listener-issue"></a>Problema del agente de escucha de escritorio remoto

Se ha detectado un problema en algunos servidores RDSH que se han actualizado directamente desde Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016. Cuando un cliente de escritorio remoto se conecta al servidor RDSH, el servidor RDSH crea un agente de escucha de escritorio remoto para la sesión de usuario. Los servidores afectados mantienen un recuento de los agentes de escucha de escritorio remoto que aumenta a medida que los usuarios conectarse, pero nunca disminuye.

Para solucionar este problema, puede usar los métodos siguientes:

  - Reinicie el servidor RDSH para restablecer el recuento de agentes de escucha de escritorio remoto
  - Modificar la directiva de límite de conexión, si se establece en un valor muy grande. Para obtener más información acerca de cómo administrar la directiva de límite de conexión, consulte [comprobación de la directiva de límite de conexión](#check-the-connection-limit-policy).

Para resolver este problema, se aplican las siguientes actualizaciones a los servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Comprobación de la directiva de límite de conexión

Puede establecer el límite del número de conexiones simultáneas de escritorio remotas en el nivel de equipo individual o mediante la configuración de un objeto de directiva de grupo (GPO). De forma predeterminada, no se establece el límite.

Para comprobar la configuración actual e identificar todos los GPO existentes en el servidor RDSH, abra una ventana del símbolo del sistema como administrador y escriba el siguiente comando:
  
```
gpresult /H c:\gpresult.html
```
   
Después de este comando finalice, gpresult.html abierto y en **configuración del equipo\\plantillas administrativas\\componentes de Windows\\servicios de escritorio remoto\\Host de sesión de escritorio remoto \\Conexiones**, busque el **limitar número de conexiones** directiva.

  - Si el valor de esta directiva es **deshabilitado**, a continuación, la directiva de grupo no es limitar las conexiones RDP.
  - Si el valor de esta directiva es **habilitado**, a continuación, compruebe **GPO prevalente**. Si tiene que quitar o cambiar el límite de conexiones, editar este GPO.

Para aplicar los cambios de directiva, abra una ventana del símbolo del sistema en el equipo afectado y escriba el siguiente comando:
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>Cliente de escritorio remoto se desconecta y no se puede volver a conectarse a la misma sesión

Después de cliente de escritorio remoto pierde su conexión a Escritorio remoto, el cliente no se puede volver a conectarse inmediatamente. El usuario recibe mensajes de error como el siguiente:

  - Debido a un error de seguridad, el cliente no pudo conectarse al Terminal server. Después de asegurarse de que ha iniciado sesión la red, intente volver a conectarse al servidor.
  - Escritorio remoto desconectado. Debido a un error de seguridad, el cliente no pudo conectarse al equipo remoto. Compruebe que inició la sesión en la red y, a continuación, intente conectarse de nuevo.

En un momento posterior, el cliente de escritorio remoto volverá a conectar con el escritorio remoto. Sin embargo, en lugar de conectarse al cliente a la sesión original, el servidor RDSH conecta al cliente a una nueva sesión. Comprobación del servidor RDSH revela que la sesión original no especifica un estado desconectado, pero en su lugar, permanece activo.

Para solucionar este problema, puede habilitar la **intervalo de configurar una conexión keep-alive** directiva en el **configuración del equipo\\plantillas administrativas\\decomponentesdeWindows\\ Servicios de escritorio remoto\\Host de sesión de escritorio remoto\\conexiones** carpeta Directiva de grupo. Si habilita esta directiva, debe especificar un intervalo keep-alive. El intervalo keep-alive determina con qué frecuencia, en minutos, el servidor comprueba el estado de sesión.

Este problema puede deberse a una configuración incorrecta de la configuración de autenticación y configuración. Puede configurar estas opciones en el nivel de servidor, o mediante el uso de los GPO. La configuración de directiva de grupo está disponible en el **configuración del equipo\\plantillas administrativas\\componentes de Windows\\servicios de escritorio remoto\\Host de sesión de escritorio remoto\\ Seguridad** carpeta Directiva de grupo.

1. En el servidor host de sesión de Escritorio remoto, abra la Configuración del host de sesión de Escritorio remoto.
2. En **conexiones**, haga clic en el nombre de la conexión y, a continuación, haga clic en **propiedades**.
3. En el **propiedades** cuadro de diálogo para la conexión, en el **General** ficha **seguridad** de capas, seleccione un método de seguridad.
4. En **nivel de cifrado**, haga clic en el nivel que desee. Puede seleccionar **baja**, **cliente Compatible**, **alta**, o **compatible con FIPS**.

> [!NOTE]  
>  - Cuando la comunicación entre clientes y servidores Host de sesión de escritorio remoto requiere el máximo nivel de cifrado, utilice el cifrado compatible con FIPS.
>  - Cualquier configuración del nivel de cifrado que establezca en directiva de grupo invalide la configuración que establece mediante la herramienta de configuración de servicios de escritorio remoto. Además, si habilita la [criptografía de sistema: Usar algoritmos que cumplan FIPS para cifrado, firma y operaciones hash](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) directiva, esta configuración invalida la **establecer el nivel de cifrado de conexión de cliente** directiva. La directiva de criptografía de sistema está en el **configuración del equipo\\Windows configuración\\configuración de seguridad\\directivas locales\\las opciones de seguridad** carpeta.
>  - Al cambiar de nivel de cifrado, el nuevo nivel de cifrado surte efecto la próxima vez un usuario inicia sesión. Si necesita varios niveles de cifrado en un servidor, instale a varios adaptadores de red y configure cada adaptador por separado.
>  - Para comprobar que el certificado tiene una clave privada correspondiente, en la configuración de servicios de escritorio remoto, haga clic en la conexión para el que desea ver el certificado, seleccione **General**, seleccione **editar**, seleccione el certificado que desea ver y, a continuación, seleccione **Ver certificado**. En la parte inferior de la **General** ficha, debe aparecer la instrucción, "tiene una clave privada que corresponde a este certificado". También puede ver esta información utilizando el complemento certificados.
>  - El cifrado compatible con FIPS (el **criptografía de sistema: Algoritmos que cumplan use FIPS para cifrado, firma y operaciones hash** directiva o la **compatible con FIPS** en configuración del servidor de escritorio remoto) cifra y descifra los datos enviados desde el cliente al servidor y desde el servidor al cliente, con los algoritmos de cifrado de 140-1 de la información de procesamiento estándar Federal (FIPS), utilizando los módulos criptográficos de Microsoft. Para obtener más información, consulte [FIPS 140 validación](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - El **alta** configuración cifra los datos enviados desde el cliente al servidor y desde el servidor al cliente mediante el uso de cifrado de 128 bits.
>  - El **cliente Compatible** configuración cifra los datos enviados entre el cliente y el servidor en el nivel máximo de clave admitido por el cliente.
>  - El **baja** configuración cifra los datos enviados desde el cliente al servidor mediante cifrado de 56 bits.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>Portátiles remotos se desconecta de la red inalámbrica

Este problema puede producirse cuando un cliente de escritorio remoto se conecta a un equipo portátil con una red inalámbrica 802.1X. De forma intermitente, el equipo portátil se desconecta de la red inalámbrica y no volver a conectar automáticamente.

Se trata de un problema conocido que se produce cuando la configuración de autenticación de red para la conexión de red inalámbrica es **autenticación de usuario**.

Para solucionar este problema, establezca la configuración de autenticación de red en **autenticación de usuario o equipo** o **autenticación del equipo**.

 > [!NOTE]  
> Para cambiar la configuración de autenticación de red en un único equipo, debe usar el panel de control de centro de redes y recursos compartidos para crear una nueva conexión inalámbrica con la nueva configuración.

Para obtener una descripción completa de cómo configurar una red inalámbrica mediante GPO, vea [directivas de redes inalámbricas (IEEE 802.11) configurar](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>Usuario experimenta un rendimiento deficiente o problemas de aplicaciones

Esta sección aborda varios problemas comunes que los usuarios pueden experimentar al usar la funcionalidad de escritorio remota:

  - [Los usuarios que se conectan a nuevas máquinas virtuales de Microsoft Azure de forma intermitente experimentan problemas](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Los usuarios que se conectan a escritorios remotos de Windows 10 versión 1709 pueden tener problemas al reproducir vídeo](#video-playback-issues-on-windows-10-version-1709).
  - [Los usuarios con perfiles de usuario de solo lectura que se conectan a escritorios remotos de Windows 10 no pueden compartir sus escritorios.](#desktop-sharing-issues-on-windows-10)
  - [Los usuarios conectarse a escritorios remotos de Windows 10 con los clientes que se ejecutan en versiones anteriores de Windows 10 experimentan un rendimiento lento cuando se deshabilita NLA](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Cuando los usuarios ejecutan varias aplicaciones en sesiones de escritorio remoto y desconectarán y volver a conectarse con frecuencia, puede encontrar una pantalla negra al volver a conectarse.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemas intermitentes con nuevas máquinas virtuales de Microsoft Azure

Este problema afecta a las máquinas virtuales que se hayan aprovisionado recientemente. Después de que el usuario se conecta a la máquina virtual, la sesión de escritorio remota no puede cargar toda la configuración del usuario correctamente.

Para solucionar este problema, desconecte la máquina virtual, espere al menos 20 minutos y, a continuación, volver a conectar.

Para resolver este problema, se aplican las siguientes actualizaciones a las máquinas virtuales, según corresponda:

  - Windows 10 y Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemas de reproducción de vídeo en Windows 10 versión 1709

Este problema se produce cuando los usuarios se conectan a equipos remotos que ejecutan Windows 10, versión 1709. Cuando estos usuarios reproducción el vídeos mediante la VMR9 códec (9 de vídeo del representador de mezcla), el Reproductor muestra una ventana en negro.

Se trata de un problema conocido en Windows 10, versión 1709. El problema no ocurre en Windows 10 versión 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Compartir problemas en Windows 10 escritorio

Este problema se produce cuando el usuario tiene un perfil de usuario de solo lectura (y el subárbol del registro asociada), como en un escenario de pantalla completa. Cuando dicho usuario se conecta a un equipo remoto que ejecuta Windows 10, versión 1803, no pueden compartir su escritorio.

Para corregir este problema, aplique la actualización de Windows 10 4340917, [24 de julio de 2018: KB4340917 (17134.191 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemas de rendimiento al mezclar versiones de Windows 10, si se deshabilita NLA

Este problema se produce cuando se deshabilita NLA y conectan los equipos cliente de escritorio remoto que ejecutan Windows 10 para escritorios remotos que ejecutan diferentes versiones de Windows 10. Los usuarios de clientes de escritorio remoto en equipos que ejecutan Windows 10, versión 1709 o versiones anteriores experimentan un rendimiento deficiente cuando intenten conectarse a escritorios remotos que ejecutan Windows 10, versión 1803 o una versión posterior.

Esto ocurre porque, cuando se deshabilita NLA, los equipos cliente anterior usan un protocolo más lento cuando se conectan a Windows 10, versión 1803 o una versión posterior.

Para resolver este problema, aplique 4340917 KB, [24 de julio de 2018: KB4340917 (17134.191 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema de la pantalla en negro

Este problema se produce en Windows 8.0, Windows 8.1, Windows 10 RTM y Windows Server 2012 R2. Un usuario inicia varias aplicaciones en un equipo de escritorio remoto y luego se desconecta de la sesión. Periódicamente, el usuario se vuelve a conectar con el escritorio remoto para interactuar con las aplicaciones y, a continuación, vuelva a desconectar. En algún momento, cuando el usuario vuelve a conectarse, la sesión de escritorio remota solo muestra una pantalla en negro. El usuario debe finalizar la sesión de escritorio remota desde la consola del equipo remoto (o la consola del servidor RDSH). Esta acción detiene las aplicaciones.

Para resolver este problema, se aplican las siguientes actualizaciones según corresponda:

  - Windows 8 y Windows Server 2012: KB4103719, [de 17 de mayo de 2018: KB4103719 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 y Windows Server 2012 R2: KB4103724, [17 de mayo de 2018: KB4103724 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) y KB 4284863, [21 de junio de 2018: KB4284863 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Se ha corregido en KB4284860, [12 de junio de 2018: KB4284860 (compilación del sistema operativo 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
