---
title: Administración de Server Core
description: Obtenga información sobre cómo administrar una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718715"
---
# <a name="manage-a-server-core-server"></a>Administrar un servidor Server Core
 
> Se aplica a: (delimitadas anuales del canal) de Windows Server y Windows Server 2016

Puede administrar un servidor Server Core de las siguientes maneras:
- Mediante el [Centro de administración de Windows](../../manage/windows-admin-center/overview.md)
- Uso de [Herramientas de administración remota del servidor](../../remote/remote-server-administration-tools.md) que se ejecutan en Windows 10
- Uso de Windows PowerShell de forma local y remota
- Con [Server Manager](../server-manager/server-manager.md) de forma remota
- Forma remota mediante un [complemento de MMC](#managing-with-microsoft-management-console)
- Forma remota con [Servicios de escritorio remoto](#managing-with-remote-desktop-services)

También puede agregar hardware y administrar controladores localmente, siempre y cuando lo hace desde la línea de comandos.

Existen algunas limitaciones importantes y sugerencias que deben tenerse en cuenta cuando se trabaja con Server Core:

- Si cierra todas las ventanas de símbolo del sistema y desea abrir una nueva ventana de símbolo del sistema, puede hacer desde el Administrador de tareas. Presione **CTRL\ + ALT\ + SUPR**, haga clic en **Iniciar el Administrador de tareas**, haga clic en **más detalles > Archivo > ejecutar**y, a continuación, escriba **cmd.exe**. (Escriba **Powershell.exe** para abrir una ventana de comandos de PowerShell). Como alternativa, puede cerrar la sesión y, a continuación, vuelva a iniciarla.
- Cualquier comando o herramienta que intenta iniciar el Explorador de Windows no funcionará. Por ejemplo, se ejecuta **iniciar.** desde un símbolo del sistema no funcionará.
- No hay ninguna compatibilidad para la representación de HTML o de la Ayuda HTML en Server Core.
- Server Core es compatible con Windows Installer en modo silencioso para que pueda instalar herramientas y utilidades de archivos de Windows Installer. Al instalar los paquetes de Windows Installer en Server Core, use la **opción/qb** para mostrar la interfaz de usuario básica.
- Para cambiar la zona horaria, ejecute **Set-fecha**.
- Para cambiar la configuración internacional, ejecute **intl.cpl de control**.
- **Control.exe** no se ejecutará en su propio. Debe ejecutarlo con **Timedate.cpl** o **Intl.cpl**.
- **Winver.exe** no está disponible en Server Core. Para obtener información sobre la versión use **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Administración de Server Core con el centro de administración de Windows
[Centro de administración de Windows](../../manage/windows-admin-center/overview.md) es una aplicación de administración basada en explorador que permite la administración local de los servidores de Windows con ninguna dependencia de Azure o en la nube. Centro de administración de Windows le ofrece control total sobre todos los aspectos de su infraestructura de servidores y es especialmente útil para la administración de las redes privadas que no están conectados a Internet. Puede instalar el centro de administración de Windows en 10 de Windows, en un servidor de puerta de enlace o en una instalación de Windows Server con experiencia de escritorio y, a continuación, conectarse al sistema básica del servidor que desea administrar.

## <a name="managing-server-core-remotely-with-server-manager"></a>Administración de forma remota con el Administrador de servidores de Server Core

Administrador de servidores es una consola de administración de Windows Server que le ayudará a aprovisionar y administrar los servidores locales y remotos basados en Windows desde sus equipos de sobremesa, sin necesidad de acceso físico a servidores, o bien la necesidad de habilitar el protocolo de escritorio remoto (RDP) conexiones a cada servidor. Administrador del servidor admite la administración remota, varios servidor.

Para habilitar el servidor local ser administrado por el Administrador de servidor que se ejecutan en un servidor remoto, ejecute el cmdlet de Windows PowerShell **Configure SMRemoting.exe – habilitar**.

## <a name="managing-with-microsoft-management-console"></a>Administración de la consola de administración de Microsoft

Puede usar muchos complementos para Microsoft Management Console (MMC) de forma remota para administrar el servidor de Server Core.

Para usar un complemento de MMC para administrar un servidor Server Core que es un miembro de dominio: 

1. Inicie un complemento de MMC, como administración de equipos.
2. Haga clic en el complemento y, a continuación, haga clic en **Conectar a otro equipo**.
2. Escriba el nombre del equipo del servidor principal del servidor y, a continuación, haga clic en **Aceptar**. Ahora puede usar el complemento de MMC para administrar el servidor de Server Core tal y como lo haría con cualquier otro PC o servidor.

Para usar un complemento de MMC para administrar un servidor Server Core es decir *no* un miembro de dominio: 

1. Establecer credenciales alternativas que se usarán para conectar con el equipo de Server Core escribiendo el siguiente comando en un símbolo del sistema en el equipo remoto:
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   Si desea que se le pida una contraseña, se omite la **opción/pasar** .

2. Cuando se le solicite, escriba la contraseña para el nombre de usuario que haya especificado.
   Si el servidor de seguridad en el servidor de Server Core ya no está configurado para permitir que los complementos de MMC para conectarse, siga los pasos siguientes para configurar el Firewall de Windows para permitir el complemento de MMC. A continuación, continúe con el paso 3.
3. En un equipo distinto, inicie un complemento de MMC, como **Administración de equipos**.
4. En el panel izquierdo, haga clic en el complemento y, a continuación, haga clic en **Conectar a otro equipo**. (Por ejemplo, en el ejemplo de administración de equipos, se debería haga clic en **Administración de equipos (Local)**.)
5. En **otro equipo**, escriba el nombre del equipo del servidor de Server Core y, a continuación, haga clic en **Aceptar**. Ahora puede usar el complemento de MMC para administrar el servidor de Server Core como lo haría con cualquier otro equipo que ejecuta un sistema operativo Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Para configurar Firewall de Windows para permitir el complemento de MMC (s) para conectarse
Para permitir que todos los complementos MMC para conectarse, ejecute el siguiente comando:

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Para permitir que solo MMC complementos específicos para conectarse, ejecute lo siguiente:
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Donde el *grupo de reglas* es uno de los siguientes, dependiendo de qué complemento que desea conectarse:

| Complemento de MMC                            | Grupo de reglas                                            |
|----------------------------------------|-------------------------------------------------------|
| Visor de eventos                           | Administración remota de registro de eventos                           |
| Services                               | Administración remota de servicios                             |
| Carpetas compartidas                         | Compartir archivos e impresoras                              |
| Programador de tareas                         | Registros de rendimiento y las alertas, compartir archivos e impresoras |
| Administración de discos                        | Administración remota del volumen                              |
| Firewall de Windows y seguridad avanzada | Administración remota de Firewall de Windows                    |


> [!NOTE] 
> Algunos complementos MMC no tienen un grupo de regla correspondiente que se les permite conectarse a través del firewall. Sin embargo, la habilitación de los grupos de reglas para el Visor de eventos, servicios o carpetas compartidas le permitirá más otros complementos para conectarse. 
>
> Además, ciertos complementos requieran una configuración adicional antes de que se pueden conectar a través de Firewall de Windows:
>
> - Administración de discos. En primer lugar debe iniciar el servicio de disco Virtual (VDS) en el equipo de Server Core. También debe configurar correctamente las reglas de administración de discos en el equipo que ejecuta el complemento de MMC.
> - Monitor de seguridad IP. En primer lugar debe habilitar la administración remota de este complemento. Para ello, en un símbolo del sistema, escriba **Cscript \windows\system32\scregedit.wsf/de IM 1**
> - Confiabilidad y rendimiento. El complemento no requiere más configuración, pero cuando se utiliza para supervisar un equipo de Server Core, solo puede supervisar los datos de rendimiento. Datos de confiabilidad no están disponibles.

## <a name="managing-with-remote-desktop-services"></a>Administración de servicios de escritorio remoto

Puede usar [Escritorio remoto](../../remote/remote-desktop-services/welcome-to-rds.md) para administrar un servidor Server Core desde equipos remotos.

Puede tener acceso a Server Core, debe ejecutar el siguiente comando: 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
Esto permite que el escritorio remoto para el modo de administración Aceptar conexiones.

## <a name="add-hardware-and-manage-drivers-locally"></a>Agregar hardware y administrar controladores localmente

Para agregar hardware para un servidor de Server Core, siga las instrucciones proporcionadas por el proveedor de hardware para la instalación de hardware nuevo. 

Si el hardware no es plug and play, debe instalar manualmente el controlador. Para ello, copie los archivos del controlador en una ubicación temporal en el servidor y, a continuación, ejecute el siguiente comando:
```
pnputil –i –a <driverinf>
```
Donde *driverinf* es el nombre de archivo del archivo .inf para el controlador.

Si se le solicita, reinicie el equipo.

Para ver qué controladores están instalados, ejecute el siguiente comando: 
```
sc query type= driver
```

> [!NOTE] 
> Debe incluir el espacio después del signo igual para el comando para llevar a cabo correctamente.

Para deshabilitar a un controlador de dispositivo, ejecute lo siguiente: 
```
sc delete <service_name>
```

Donde *service_name* es el nombre del servicio que recibió al ejecutar **tipo de consulta de sc = driver**.