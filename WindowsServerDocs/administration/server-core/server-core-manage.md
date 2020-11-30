---
title: Administrar Server Core
description: Más información sobre cómo administrar una instalación Server Core de Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 07/23/2019
ms.openlocfilehash: 754211824208e582382cb9c6ad196483c6506708
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330387"
---
# <a name="manage-a-server-core-server"></a>Administrar un servidor Server Core
 
> Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Puede administrar un servidor Server Core de las siguientes maneras:
- Usar el [centro de administración de Windows](../../manage/windows-admin-center/overview.md)
- Uso de [herramientas de administración remota del servidor](../../remote/remote-server-administration-tools.md) que se ejecutan en Windows 10
- De forma local y remota con Windows PowerShell
- De forma remota con [Administrador del servidor](../server-manager/server-manager.md)
- De forma remota con un [complemento MMC](#managing-with-microsoft-management-console)
- De forma remota con [servicios de escritorio remoto](#managing-with-remote-desktop-services)

También puede agregar hardware y administrar controladores de forma local, siempre que lo haga desde la línea de comandos.

Hay algunas limitaciones y sugerencias importantes que se deben tener en cuenta al trabajar con Server Core:

- Si cierra todas las ventanas del símbolo del sistema y desea abrir una nueva ventana del símbolo del sistema, puede hacerlo desde el administrador de tareas. Presione **Ctrl \+ ALT \+ Supr**, haga clic en **iniciar el administrador de tareas**, haga clic en **más detalles > archivo > ejecutar** y, a continuación, escriba **cmd.exe**. (Escriba **Powershell.exe** para abrir una ventana de comandos de PowerShell). También puede cerrar la sesión y volver a iniciarla.
- Ningún comando o herramienta que intente iniciar el Explorador de Windows funcionará. Por ejemplo, ejecutar **Start.** desde un símbolo del sistema, no funcionará.
- No se admite la representación en HTML ni la ayuda HTML en Server Core.
- Server Core admite Windows Installer en modo silencioso, por lo que puede instalar herramientas y utilidades desde archivos Windows Installer. Al instalar Windows Installer paquetes en Server Core, use la opción **/qb** para mostrar la interfaz de usuario básica.
- Para cambiar la zona horaria, ejecute **set-date**.
- Para cambiar la configuración internacional, ejecute el **intl.cplde control**.
- **Control.exe** no se ejecutará por sí mismo. Debe ejecutarlo con **Timedate.cpl** o **Intl.cpl**.
- **Winver.exe** no está disponible en Server Core. Para obtener información de versión, use **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Administrar Server Core con el centro de administración de Windows
El [centro de administración de Windows](../../manage/windows-admin-center/overview.md) es una aplicación de administración basada en explorador que permite la administración local de servidores de Windows sin dependencias de Azure o de la nube. El centro de administración de Windows ofrece control total sobre todos los aspectos de la infraestructura de servidor y es especialmente útil para la administración en redes privadas que no están conectadas a Internet. Puede instalar el centro de administración de Windows en Windows 10, en un servidor de puerta de enlace o en una instalación de Windows Server con experiencia de escritorio y, a continuación, conectarse al sistema Server Core que desea administrar.

## <a name="managing-server-core-remotely-with-server-manager"></a>Administrar Server Core de forma remota con Administrador del servidor

Administrador del servidor es una consola de administración de Windows Server que le ayuda a aprovisionar y administrar servidores basados en Windows, tanto locales como remotos, desde sus escritorios, sin necesidad de tener acceso físico a los servidores ni de habilitar conexiones de protocolo de Escritorio remoto (RDP) a cada servidor. Administrador del servidor admite la administración remota de varios servidores.

Para permitir que el servidor local se administre mediante Administrador del servidor que se ejecuta en un servidor remoto, ejecute el cmdlet de Windows PowerShell **Configure-SMRemoting.exe-enable**.

## <a name="managing-with-microsoft-management-console"></a>Administración con Microsoft Management Console

Puede usar muchos complementos de Microsoft Management Console (MMC) de forma remota para administrar el servidor Server Core.

Para usar un complemento MMC para administrar un servidor Server Core que sea miembro de dominio:

1. Inicie un complemento MMC, como Administración de equipos.
2. Haga clic con el botón secundario en el complemento y, a continuación, haga clic en **conectar a otro equipo**.
2. Escriba el nombre de equipo del servidor Server Core y, a continuación, haga clic en **Aceptar**. Ahora puede usar el complemento MMC para administrar el servidor Server Core como haría con cualquier otro equipo o servidor.

Para usar un complemento MMC para administrar un servidor Server Core que *no* sea miembro de dominio:

1. Establezca credenciales alternativas para conectarse al equipo Server Core escribiendo el siguiente comando en un símbolo del sistema en el equipo remoto:

   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   Si desea que se le solicite una contraseña, omita la opción **/Pass** .

2. Cuando se le solicite, escriba la contraseña del nombre de usuario que especificó.
   Si el firewall del servidor Server Core aún no está configurado para permitir que los complementos MMC se conecten, siga los pasos que se indican a continuación para configurar Firewall de Windows para permitir el complemento MMC. Después, continúe con el paso 3.
3. En otro equipo, inicie un complemento MMC, como **Administración de equipos**.
4. En el panel izquierdo, haga clic con el botón secundario en el complemento y, a continuación, haga clic en **conectar a otro equipo**. (Por ejemplo, en el ejemplo de administración de equipos, haría clic con el botón secundario en **Administración de equipos (local)**).
5. En **otro equipo**, escriba el nombre de equipo del servidor Server Core y, a continuación, haga clic en **Aceptar**. Ahora ya puede usar el complemento MMC para administrar el servidor Server Core como haría con cualquier otro equipo que ejecute un sistema operativo Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Para configurar el Firewall de Windows para permitir conectarse a los complementos MMC
Para permitir que todos los complementos MMC se conecten, ejecute el siguiente comando:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "Windows Remote Management"
```

Para permitir que solo se conecten determinados complementos de MMC, ejecute lo siguiente:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Donde *rulegroup* es uno de los siguientes, en función del complemento que desee conectar:

| Complemento MMC                            | Grupo de reglas                                            |
| ---------------------------------------- | ------------------------------------------------------- |
| Visor de eventos                           | Administración remota de registro de eventos                           |
| Servicios                               | Administración remota de servicios                             |
| Carpetas compartidas                         | Uso compartido de archivos e impresoras                              |
| Programador de tareas                         | Registros y alertas de rendimiento, uso compartido de archivos e impresoras |
| Administración de discos                        | Administración remota del volumen                              |
| Firewall de Windows y seguridad avanzada | Administración remota de Firewall de Windows                    |


> [!NOTE]
> Algunos complementos MMC no tienen un grupo de reglas correspondiente que les permita conectarse a través del firewall. Sin embargo, habilitar los grupos de reglas del Visor de eventos, Servicios o Carpetas compartidas permitirá que la mayoría de complementos se conecten.
>
> Además, ciertos complementos requieren una mayor configuración antes de poder conectarse a través del Firewall de Windows:
>
> - Administración de discos. Primero debe iniciar el Servicio de disco virtual (VDS) en el equipo Server Core. También debe configurar las reglas de Administración de discos de forma adecuada en el equipo que ejecuta el complemento MMC.
> - Monitor de seguridad IP. Primero debe habilitar la administración remota de este complemento. Para ello, en un símbolo del sistema, escriba **cscript c:\windows\system32\scregedit.wsf/im 1**
> - Confiabilidad y rendimiento. El complemento no requiere ninguna configuración adicional, pero cuando lo use para supervisar un equipo Server Core, solo podrá supervisar datos de rendimiento. Los datos de confiabilidad no están disponibles.

## <a name="managing-with-remote-desktop-services"></a>Administrar con Servicios de Escritorio remoto

Puede usar [escritorio remoto](../../remote/remote-desktop-services/welcome-to-rds.md) para administrar un servidor Server Core desde equipos remotos.

Antes de poder acceder a Server Core, deberá ejecutar el siguiente comando:

```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```

Esto habilita el modo Escritorio remoto para Administración para aceptar conexiones.

## <a name="add-hardware-and-manage-drivers-locally"></a>Agregar hardware y administrar controladores de forma local

Para agregar hardware a un servidor Server Core, siga las instrucciones proporcionadas por el proveedor de hardware para instalar el nuevo hardware.

Si el hardware no es plug and Play, deberá instalar manualmente el controlador. Para ello, copie los archivos del controlador en una ubicación temporal del servidor y, a continuación, ejecute el siguiente comando:

```
pnputil –i –a <driverinf>
```

Donde *driverinf* es el nombre de archivo del archivo. inf para el controlador.

Si se le solicite, reinicie el equipo.

Para ver qué controladores están instalados, ejecute el siguiente comando:

```
sc query type= driver
```

> [!NOTE]
> Debe incluir el espacio después del signo igual para que el comando se complete correctamente.

Para deshabilitar un controlador de dispositivo, ejecute lo siguiente:

```
sc delete <service_name>
```

Donde *SERVICE_NAME* es el nombre del servicio que recibió al ejecutar **sc query Type = driver**.
