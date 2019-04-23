---
title: Administrar Server Core
description: Obtenga información sobre cómo administrar una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869306"
---
# <a name="manage-a-server-core-server"></a>Administrar un servidor server Core
 
> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Puede administrar un servidor server Core de las maneras siguientes:
- Uso de [Windows Admin Center](../../manage/windows-admin-center/overview.md)
- Uso de [herramientas de administración remota del servidor](../../remote/remote-server-administration-tools.md) que se ejecutan en Windows 10
- De forma local y remota con Windows PowerShell
- Uso de forma remota [administrador del servidor](../server-manager/server-manager.md)
- Uso de forma remota un [complemento MMC](#managing-with-microsoft-management-console)
- Remotamente con [servicios de escritorio remoto](#managing-with-remote-desktop-services)

También puede agregar hardware y administrar controladores de forma local, siempre que hacerlo desde la línea de comandos.

Hay algunas limitaciones importantes y sugerencias a tener en cuenta cuando se trabaja con Server Core:

- Si cierra todas las ventanas de símbolo del sistema y desea abrir una nueva ventana de símbolo del sistema, puede hacerlo desde el Administrador de tareas. Presione **CTRL\+ALT\+eliminar**, haga clic en **iniciar Administrador de tareas**, haga clic en **más detalles > Archivo > ejecutar**y, a continuación, escriba  **cmd.exe**. (Tipo **Powershell.exe** para abrir una ventana de comandos de PowerShell.) Como alternativa, puede cerrar la sesión y vuelva a iniciar sesión.
- Ningún comando o herramienta que intente iniciar el Explorador de Windows funcionará. Por ejemplo, ejecutar **iniciar.** desde un símbolo del sistema no funcionará.
- No hay ninguna compatibilidad para la representación en HTML o de la Ayuda HTML en Server Core.
- Server Core es compatible con Windows Installer en modo silencioso para que pueda instalar las herramientas y utilidades desde archivos de Windows Installer. Cuando instale paquetes de Windows Installer en Server Core, use el **/qb** opción para mostrar la interfaz de usuario básica.
- Para cambiar la zona horaria, ejecute **Set-Date**.
- Para cambiar la configuración internacional, ejecute **control intl.cpl**.
- **Control.exe** no se ejecuta en su propio. Debe ejecutarlo con cualquiera **Timedate.cpl** o **Intl.cpl**.
- **Winver.exe** no está disponible en Server Core. Para obtener información de versión, use **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Administración de Server Core con Windows Admin Center
[Windows Admin Center](../../manage/windows-admin-center/overview.md) es una aplicación de administración basada en explorador que permite la administración local de los servidores de Windows sin ninguna dependencia de Azure o la nube. Windows Admin Center te ofrece el control total sobre todos los aspectos de la infraestructura de servidores y es especialmente útil para la administración de las redes privadas que no están conectadas a Internet. Puede instalar Windows Admin Center en Windows 10, en un servidor de puerta de enlace o en una instalación de Windows Server con experiencia de escritorio y, a continuación, conectarse al sistema Server Core que desea administrar.

## <a name="managing-server-core-remotely-with-server-manager"></a>Administración de forma remota con Administrador del servidor de Server Core

Administrador del servidor es una consola de administración de Windows Server que le permite aprovisionar y administrar los servidores locales y remotos basados en Windows desde sus escritorios, sin requerir acceso físico a los servidores o la necesidad de habilitar el protocolo de escritorio remoto (RDP) conexiones a cada servidor. El administrador del servidor admite la administración remota de varios servidor.

Para habilitar su servidor local para ser administrada por el Administrador de servidor que se ejecutan en un servidor remoto, ejecute el cmdlet de Windows PowerShell **Configure-SMRemoting.exe-habilitar**.

## <a name="managing-with-microsoft-management-console"></a>Administración con Microsoft Management Console

Puede usar muchos complementos para Microsoft Management Console (MMC) de forma remota para administrar el servidor Server Core.

Para usar un complemento de MMC para administrar un servidor server Core, es miembro del dominio: 

1. Inicie un complemento de MMC, como administración de equipos.
2. Haga clic en el complemento y, a continuación, haga clic en **conectarse a otro equipo**.
2. Escriba el nombre del equipo del servidor Server Core y, a continuación, haga clic en **Aceptar**. Ahora puede usar el complemento de MMC para administrar el servidor server Core tal y como lo haría con cualquier otro equipo o servidor.

Para usar un complemento de MMC para administrar un servidor Server Core que esté *no* un miembro del dominio: 

1. Establecer credenciales alternativas para conectarse al equipo Server Core escribiendo el siguiente comando en un símbolo del sistema en el equipo remoto:
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   Si desea que se le pida una contraseña, omita el **/pasar** opción.

2. Cuando se le solicite, escriba la contraseña para el nombre de usuario especificado.
   Si el firewall en el servidor server Core ya no está configurado para permitir que los complementos MMC para conectarse, siga estos pasos para configurar Firewall de Windows para permitir el complemento de MMC. A continuación, continúe con el paso 3.
3. En un equipo diferente, inicie un complemento de MMC, como **administración de equipos**.
4. En el panel izquierdo, haga clic en el complemento y, a continuación, haga clic en **conectarse a otro equipo**. (Por ejemplo, en el ejemplo de administración de equipos, haga **administración de equipos (Local)**.)
5. En **otro equipo**, escriba el nombre del equipo del servidor Server Core y, a continuación, haga clic en **Aceptar**. Ahora ya puede usar el complemento MMC para administrar el servidor Server Core como haría con cualquier otro equipo que ejecute un sistema operativo Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Para configurar el Firewall de Windows para permitir conectarse a los complementos MMC
Para permitir que todos los complementos MMC para conectarse, ejecute el siguiente comando:

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Para permitir que solo específicos complementos MMC para conectarse, ejecute lo siguiente:
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Donde *rulegroup* es uno de los siguientes, dependiendo de complemento que desea conectarse:

| Complemento MMC                            | Grupo de reglas                                            |
|----------------------------------------|-------------------------------------------------------|
| Visor de eventos                           | Administración remota de registro de eventos                           |
| Servicios                               | Administración remota de servicios                             |
| Carpetas compartidas                         | Uso compartido de archivos e impresoras                              |
| Programador de tareas                         | Los registros de rendimiento y alertas, compartir archivos e impresoras |
| Administración de discos                        | Administración remota del volumen                              |
| Firewall de Windows y seguridad avanzada | Administración remota de Firewall de Windows                    |


> [!NOTE] 
> Algunos complementos MMC no tienen un grupo de reglas correspondiente que les permita conectarse a través del firewall. Sin embargo, habilitar los grupos de reglas del Visor de eventos, Servicios o Carpetas compartidas permitirá que la mayoría de complementos se conecten. 
>
> Además, ciertos complementos requieren una mayor configuración antes de poder conectarse a través del Firewall de Windows:
>
> - Administración de discos. Primero debe iniciar el Servicio de disco virtual (VDS) en el equipo Server Core. También debe configurar las reglas de Administración de discos de forma adecuada en el equipo que ejecuta el complemento MMC.
> - Monitor de seguridad IP. Primero debe habilitar la administración remota de este complemento. Para ello, en un símbolo del sistema, escriba **Cscript \windows\system32\scregedit.wsf /im 1**
> - Confiabilidad y rendimiento. El complemento no requiere ninguna configuración adicional, pero cuando lo use para supervisar un equipo Server Core, solo podrá supervisar datos de rendimiento. Los datos de confiabilidad no están disponibles.

## <a name="managing-with-remote-desktop-services"></a>Administración de servicios de escritorio remoto

Puede usar [escritorio remoto](../../remote/remote-desktop-services/welcome-to-rds.md) para administrar un servidor server Core desde equipos remotos.

Antes de poder acceder a Server Core, deberá ejecutar el comando siguiente: 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
Esto habilita el modo Escritorio remoto para Administración para aceptar conexiones.

## <a name="add-hardware-and-manage-drivers-locally"></a>Agregar hardware y administrar controladores de forma local

Para agregar hardware a un servidor server Core, siga las instrucciones proporcionadas por el fabricante del hardware para instalar el nuevo hardware. 

Si el hardware no es plug and play, deberá instalar manualmente el controlador. Para ello, copie los archivos del controlador en una ubicación temporal en el servidor y, a continuación, ejecute el siguiente comando:
```
pnputil –i –a <driverinf>
```
Donde *driverinf* es el nombre de archivo del archivo .inf del controlador.

Si se le pide, reinicie el equipo.

Para ver qué controladores están instalados, ejecute el siguiente comando: 
```
sc query type= driver
```

> [!NOTE] 
> Debe incluir el espacio después del signo igual para que el comando se complete correctamente.

Para deshabilitar a un controlador de dispositivo, ejecute lo siguiente: 
```
sc delete <service_name>
```

Donde *service_name* es el nombre del servicio que obtuvo al ejecutar **tipo de consulta de sc = driver**.