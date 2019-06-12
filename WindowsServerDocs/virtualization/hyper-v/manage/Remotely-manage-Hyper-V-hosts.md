---
title: Administrar hosts de Hyper-V de forma remota
description: Describe la compatibilidad de versiones entre los hosts de Hyper-V y Administrador de Hyper-V y cómo conectarse a los hosts remotos en diferentes entornos, incluidos entre dominios e independiente.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: d4d9f2dd3727e196bb6893fd5041fa3f08c30796
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453180"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Administrar hosts de Hyper-V con el Administrador de Hyper-V de forma remota

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1

En este artículo se enumera las combinaciones admitidas de hosts de Hyper-V y las versiones del Administrador de Hyper-V y se describe cómo conectarse a los hosts de Hyper-V locales y remotos, por lo que puede administrarlos. 

Administrador de Hyper-V le permite administrar un pequeño número de hosts de Hyper-V locales y remotos. Se instala al instalar las herramientas de administración de Hyper-V, lo que puede hacer a través de una completa la instalación de Hyper-V o solo de herramientas. Realizar un medio de instalación solo de herramientas puede usar las herramientas en los equipos que no cumplen los requisitos de hardware para el host de Hyper-V. Para obtener más información acerca del hardware para hosts de Hyper-V, consulte [requisitos del sistema](../System-requirements-for-Hyper-V-on-Windows.md).

Si no está instalado el Administrador de Hyper-V, consulte el [instrucciones](#install-hyper-v-manager) a continuación.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinaciones admitidas del Administrador de Hyper-V y las versiones de host de Hyper-V

En algunos casos puede usar una versión diferente del Administrador de Hyper-V a la versión de Hyper-V en el host, como se muestra en la tabla. Al hacerlo, el Administrador de Hyper-V proporciona las características disponibles para la versión de Hyper-V en el host que se está administrando. Por ejemplo, si usa la versión del Administrador de Hyper-V en Windows Server 2012 R2 para administrar de forma remota un host que ejecuta Hyper-V en Windows Server 2012, no podrá usar las características disponibles en Windows Server 2012 R2 en ese host de Hyper-V.

La siguiente tabla muestra qué versiones de un host de Hyper-V se pueden administrar desde una versión determinada de Hyper-V Manager. Solo admite el sistema operativo que se enumeran las versiones. Para obtener más información sobre la compatibilidad con el estado de una versión de sistema operativo determinado, utilice el **ciclo de vida de producto de búsqueda** situado en la [Policy Microsoft Lifecycle](https://support.microsoft.com/lifecycle) página. En general, las versiones anteriores de Hyper-V Manager solo pueden administrar un host de Hyper-V que ejecutan la misma versión o la versión de Windows Server comparable.

|Versión del Administrador de Hyper-V | Versión del host de Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016, todas las ediciones y opciones de instalación, incluidas Nano Server y la versión correspondiente de Hyper-V Server <br>-Windows Server 2012 R2: todas las ediciones y opciones de instalación y la versión correspondiente de Hyper-V Server <br>-Windows Server 2012: todas las ediciones y opciones de instalación y la versión correspondiente de Hyper-V Server <br> -Windows 10 <br> -Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2: todas las ediciones y opciones de instalación y la versión correspondiente de Hyper-V Server <br>-Windows Server 2012: todas las ediciones y opciones de instalación y la versión correspondiente de Hyper-V Server <br>-Windows 8.1
| Windows Server 2012 | -Windows Server 2012: todas las ediciones y opciones de instalación y la versión correspondiente de Hyper-V Server
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | -Windows Server 2008 R2: todas las ediciones y opciones de instalación y la versión correspondiente de Hyper-V Server
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008, todas las ediciones y opciones de instalación y la versión correspondiente de Hyper-V Server

>[!NOTE]
>Compatibilidad con paquete de servicio finalizó para Windows 8 de 12 de enero de 2016. Para obtener más información, consulte el [preguntas más frecuentes de Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Conectarse a un host de Hyper-V

Para conectarse a un host de Hyper-V desde el Administrador de Hyper-V, haga clic en **Administrador de Hyper-V** en el panel izquierdo y, a continuación, haga clic en **conectar al servidor**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Administrar Hyper-V en un equipo local

Administrador de Hyper-V no se muestran todos los equipos que hospedan Hyper-V hasta que agregue el equipo, incluido un equipo local. Para ello:

1. En el panel izquierdo, haga clic en **Administrador de Hyper-V**.
2. Haga clic en **conectarse al servidor**.
3. Desde **Seleccionar equipo**, haga clic en **ordenador** y, a continuación, haga clic en **Aceptar**.

Si no se puede conectar:

* Es posible que solo las herramientas de Hyper-V están instaladas. Para comprobar que está instalada la plataforma de Hyper-V, busque el servicio de administración de máquinas virtuales. / (Abrir la aplicación de servicios de escritorio: haga clic en **inicio**, haga clic en el **Iniciar búsqueda** , escriba **services.msc**y, a continuación, presione **ENTRAR**. Si no aparece el servicio de administración de máquinas virtuales, instalar la plataforma de Hyper-V, siga las instrucciones en [instalar Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).
* Compruebe que el hardware cumple los requisitos. Consulte [requisitos del sistema](../System-requirements-for-Hyper-V-on-Windows.md).
* Compruebe que la cuenta de usuario pertenece al grupo Administradores o del grupo Administradores de Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Administrar hosts de Hyper-V de forma remota  

Para administrar hosts remotos de Hyper-V, habilitar la administración remota en el equipo local y el host remoto.

En Windows Server, abra el administrador del servidor \> **servidor Local** \> **administración remota** y, a continuación, haga clic en **permitir conexiones remotas con este equipo**. 

O bien, desde cualquiera de los sistemas operativos, abra Windows PowerShell como administrador y ejecute: 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Conectarse a los hosts en el mismo dominio

Windows 8.1 y versiones anteriores, administración remota solo funciona cuando el host está en el mismo dominio y también es su cuenta de usuario local en el host remoto.

Para agregar un host de Hyper-V remoto al administrador de Hyper-V, seleccione **otro equipo** en el **Seleccionar equipo** cuadro de diálogo y escriba el nombre de host del host remoto, nombre NetBIOS o nombre de dominio completo \(FQDN\).

Administrador de Hyper-V en Windows Server 2016 y Windows 10 ofrece más tipos de conexión remota que en versiones anteriores, que se describe en las secciones siguientes.  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Conectarse a un host remoto de Windows 2016 o Windows 10 como un usuario diferente

Esto le permite conectar con el host de Hyper-V cuando no está ejecutando en el equipo local como un usuario que sea miembro del grupo Administradores de Hyper-V o del grupo Administradores en el host de Hyper-V. Para ello:

1. En el panel izquierdo, haga clic en **Administrador de Hyper-V**.
1. Haga clic en **conectarse al servidor**.
1. Seleccione **conectarse como otro usuario** en el **Seleccionar equipo** cuadro de diálogo.
1. Seleccione **establecer usuario**.

>[!NOTE]
> Esto solo funciona para Windows Server 2016 o Windows 10 **remoto** hosts.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Conectarse a un host remoto de Windows 2016 o Windows 10 con dirección IP

Para ello:

1. En el panel izquierdo, haga clic en **Administrador de Hyper-V**.
1. Haga clic en **conectarse al servidor**.
1. Escriba la dirección IP en el **otro equipo** campo de texto.

>[!NOTE]
> Esto solo funciona para Windows Server 2016 o Windows 10 **remoto** hosts.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Conectarse a un host remoto Windows 2016 o Windows 10 fuera de su dominio, o con ningún dominio

Para ello:

1. En el host de Hyper-V para administrarlos, abra una sesión de Windows PowerShell como administrador.

1. Crear las reglas de firewall necesarias para las zonas de red privada:   
   
   ```
   Enable-PSRemoting
   ```

2. Para permitir el acceso remoto en zonas públicas, habilitar las reglas de firewall para CredSSP y WinRM:
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    Para obtener más información, consulte [Enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) y [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

A continuación, configure el equipo que se va a usar para administrar el host de Hyper-V.

1. Abra una sesión de Windows PowerShell como administrador.
1. Ejecute estos comandos:

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. Es posible que también deberá configurar la directiva de grupo siguientes: 
    * **Configuración del equipo** \> **plantillas administrativas** \> **sistema** \> **ladelegacióndecredenciales** \> **Permitir delegar credenciales nuevas con autenticación de servidor solo NTLM**
    * Haga clic en **habilitar** y agregue *wsman/fqdn-de-hyper-v-host*.
1. Abra **Administrador de Hyper-V**.
1. En el panel izquierdo, haga clic en **Administrador de Hyper-V**.
1. Haga clic en **conectarse al servidor**.

>[!NOTE]
> Esto solo funciona para Windows Server 2016 o Windows 10 **remoto** hosts.

Para información sobre el cmdlet, consulte [Set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item) y [Enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

## <a name="install-hyper-v-manager"></a>Instalar a Administrador de Hyper-V

Para usar una herramienta de interfaz de usuario, elija uno adecuado para el sistema operativo en el equipo donde se va a ejecutar Hyper-V Manager:

En Windows Server, abra el administrador del servidor \> **administrar** \> **agregar roles y características**. Mover a la **características** página y expanda **herramientas de administración remota del servidor** \> **herramientas de administración de roles** \>  **Las herramientas de administración de Hyper-V**. 

En Windows, Administrador de Hyper-V está disponible en [cualquier sistema operativo Windows que incluya Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility).

1. En el escritorio de Windows, haga clic en el botón Inicio y comience a escribir **programas y características**. 
1. En los resultados de búsqueda, haga clic en **programas y características**.
1. En el panel izquierdo, haga clic en **o desactivar las características de Windows Active**.
1. Expanda la carpeta de Hyper-V y **haga clic en herramientas de administración de Hyper-V**.
1. Para instalar el Administrador de Hyper-V, haga clic en **las herramientas de administración de Hyper-V**. Si desea instalar también el módulo de Hyper-V, haga clic en esa opción.

Para usar Windows PowerShell, ejecute el siguiente comando como administrador:

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>Vea también  
 
[Instalar Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

