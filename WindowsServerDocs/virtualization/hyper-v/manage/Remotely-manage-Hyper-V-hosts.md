---
title: Administrar hosts de Hyper-V de forma remota
description: Describe la compatibilidad de versiones entre los hosts de Hyper-V y el administrador de Hyper-V, y cómo conectarse a hosts remotos en entornos diferentes, incluidos entre dominios y independientes.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d34e98c-6134-479b-8000-3eb360b8b8a3
author: KBDAzure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: 677e054fe42978697ef786b73daac75069f0408f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392690"
---
# <a name="remotely-manage-hyper-v-hosts-with-hyper-v-manager"></a>Administrar hosts de Hyper-V de forma remota con el administrador de Hyper-V

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1

En este artículo se enumeran las combinaciones admitidas de los hosts de Hyper-V y las versiones del administrador de Hyper-v y se describe cómo conectarse a los hosts de Hyper-V locales y remotos para que pueda administrarlos. 

El administrador de Hyper-V permite administrar un pequeño número de hosts de Hyper-V, tanto remotos como locales. Se instala al instalar las herramientas de administración de Hyper-V, lo que puede hacer mediante una instalación completa de Hyper-V o una instalación de solo herramientas. Realizar una instalación de solo herramientas significa que puede usar las herramientas en equipos que no cumplen los requisitos de hardware para hospedar Hyper-V. Para obtener más información sobre el hardware de los hosts de Hyper-V, consulte [requisitos del sistema](../System-requirements-for-Hyper-V-on-Windows.md).

Si el administrador de Hyper-V no está instalado, consulte las [instrucciones](#install-hyper-v-manager) siguientes.

## <a name="supported-combinations-of-hyper-v-manager-and-hyper-v-host-versions"></a>Combinaciones admitidas del administrador de Hyper-V y las versiones del host de Hyper-V

En algunos casos, puede usar una versión diferente del administrador de Hyper-V que la versión de Hyper-V en el host, tal como se muestra en la tabla. Al hacerlo, el administrador de Hyper-V proporciona las características disponibles para la versión de Hyper-V en el host que está administrando. Por ejemplo, si usa la versión del administrador de Hyper-V en Windows Server 2012 R2 para administrar de forma remota un host que ejecuta Hyper-V en Windows Server 2012, no podrá usar las características disponibles en Windows Server 2012 R2 en ese host de Hyper-V.

En la tabla siguiente se muestran las versiones de un host de Hyper-V que se pueden administrar desde una versión concreta del administrador de Hyper-V. Solo se muestran las versiones de sistema operativo compatibles. Para obtener más información sobre el estado de compatibilidad de una versión de sistema operativo determinada, use el botón **Buscar producto vida** en la página de la [Directiva del ciclo de vida de Microsoft](https://support.microsoft.com/lifecycle) . En general, las versiones anteriores del administrador de Hyper-V solo pueden administrar un host de Hyper-V que ejecute la misma versión o la versión comparable de Windows Server.

|Versión del administrador de Hyper-V | Versión del host de Hyper-V|
|---|---|
|Windows 2016, Windows 10|-Windows Server 2016: todas las ediciones y opciones de instalación, incluido nano Server, y la versión correspondiente de Hyper-V Server <br>-Windows Server 2012 R2: todas las ediciones y opciones de instalación, así como la versión correspondiente de Hyper-V Server <br>-Windows Server 2012: todas las ediciones y opciones de instalación, así como la versión correspondiente de Hyper-V Server <br> -Windows 10 <br> -Windows 8.1  |
| Windows Server 2012 R2, Windows 8.1 | -Windows Server 2012 R2: todas las ediciones y opciones de instalación, así como la versión correspondiente de Hyper-V Server <br>-Windows Server 2012: todas las ediciones y opciones de instalación, así como la versión correspondiente de Hyper-V Server <br>-Windows 8.1
| Windows Server 2012 | -Windows Server 2012: todas las ediciones y opciones de instalación, así como la versión correspondiente de Hyper-V Server
| Windows Server 2008 R2 Service Pack 1, Windows 7 Service Pack 1 | -Windows Server 2008 R2: todas las ediciones y opciones de instalación, así como la versión correspondiente de Hyper-V Server
| Windows Server 2008, Windows Vista Service Pack 2 | -Windows Server 2008: todas las ediciones y opciones de instalación, así como la versión correspondiente de Hyper-V Server

>[!NOTE]
>Compatibilidad con Service Pack finalizada para Windows 8 el 12 de enero de 2016. Para obtener más información, vea las [preguntas más frecuentes sobre Windows 8.1](https://support.microsoft.com/help/18581).

## <a name="connect-to-a-hyper-v-host"></a>Conectarse a un host de Hyper-V

Para conectarse a un host de Hyper-V desde el administrador de Hyper-V, haga clic con el botón secundario en **Administrador de Hyper-v** en el panel izquierdo y, a continuación, haga clic en **conectar al servidor**.

## <a name="manage-hyper-v-on-a-local-computer"></a>Administrar Hyper-V en un equipo local

El administrador de Hyper-V no muestra los equipos que hospedan Hyper-V hasta que se agrega el equipo, incluido un equipo local. Para ello:

1. En el panel izquierdo, haga clic con el botón secundario en **Administrador de Hyper-V**.
2. Haga clic en **conectar al servidor**.
3. En **seleccionar equipo**, haga clic en **equipo local** y, a continuación, haga clic en **Aceptar**.

Si no puede conectarse:

* Es posible que solo estén instaladas las herramientas de Hyper-V. Para comprobar que la plataforma Hyper-V está instalada, busque el servicio administración de máquinas virtuales. /(Abrir la aplicación de escritorio servicios: haga clic en **Inicio**, haga clic en el cuadro **Iniciar búsqueda** , escriba **Services. msc**y, a continuación, presione **entrar**. Si no aparece el servicio de administración de máquinas virtuales, instale la plataforma Hyper-V siguiendo las instrucciones de [instalación de Hyper-v](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).
* Compruebe que el hardware cumple los requisitos. Consulte [requisitos del sistema](../System-requirements-for-Hyper-V-on-Windows.md).
* Compruebe que la cuenta de usuario pertenece al grupo administradores o al grupo administradores de Hyper-V.

## <a name="manage-hyper-v-hosts-remotely"></a>Administrar hosts de Hyper-V de forma remota  

Para administrar hosts remotos de Hyper-V, habilite la administración remota en el equipo local y el host remoto.

En Windows Server, abra Administrador del servidor \>**servidor Local** \>**administración remota** y, a continuación, haga clic en **permitir conexiones remotas a este equipo**. 

O bien, desde cualquier sistema operativo, abra Windows PowerShell como administrador y ejecute: 

```
Enable-PSRemoting
```

### <a name="connect-to-hosts-in-the-same-domain"></a>Conexión a hosts en el mismo dominio

Para Windows 8.1 y versiones anteriores, la administración remota solo funciona cuando el host está en el mismo dominio y la cuenta de usuario local también está en el host remoto.

Para agregar un host remoto de Hyper-V al administrador de Hyper-V, seleccione **otro equipo** en el cuadro de diálogo **seleccionar equipo** y escriba el nombre de host del host remoto, el nombre NetBIOS o el nombre de dominio completo \(FQDN\).

El administrador de Hyper-V en Windows Server 2016 y Windows 10 ofrece más tipos de conexión remota que las versiones anteriores, que se describen en las secciones siguientes.  

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-as-a-different-user"></a>Conexión a un host remoto de Windows 2016 o Windows 10 como un usuario diferente

Esto le permite conectarse al host de Hyper-V cuando no se está ejecutando en el equipo local como un usuario que sea miembro del grupo administradores de Hyper-V o del grupo administradores en el host de Hyper-V. Para ello:

1. En el panel izquierdo, haga clic con el botón secundario en **Administrador de Hyper-V**.
1. Haga clic en **conectar al servidor**.
1. Seleccione **conectar como otro usuario** en el cuadro de diálogo **seleccionar equipo** .
1. Seleccione **establecer usuario**.

>[!NOTE]
> Esto solo funcionará para hosts **remotos** de windows Server 2016 o Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-using-ip-address"></a>Conexión a un host remoto de Windows 2016 o Windows 10 mediante la dirección IP

Para ello:

1. En el panel izquierdo, haga clic con el botón secundario en **Administrador de Hyper-V**.
1. Haga clic en **conectar al servidor**.
1. Escriba la dirección IP en el campo de texto **otro equipo** .

>[!NOTE]
> Esto solo funcionará para hosts **remotos** de windows Server 2016 o Windows 10.

### <a name="connect-to-a-windows-2016-or-windows-10-remote-host-outside-your-domain-or-with-no-domain"></a>Conexión a un host remoto de Windows 2016 o Windows 10 fuera del dominio o sin dominio

Para ello:

1. En el host de Hyper-V que se va a administrar, abra una sesión de Windows PowerShell como administrador.

1. Cree las reglas de Firewall necesarias para las zonas de red privada:   
   
   ```
   Enable-PSRemoting
   ```

2. Para permitir el acceso remoto en zonas públicas, habilite las reglas de Firewall para CredSSP y WinRM:
  
   ```
   Enable-WSManCredSSP -Role server
   ```

    Para obtener más información, vea [enable-PSRemoting](https://technet.microsoft.com/library/hh849694.aspx) y [enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

A continuación, configure el equipo que va a usar para administrar el host de Hyper-V.

1. Abra una sesión de Windows PowerShell como administrador.
1. Ejecute estos comandos:

     ```
     Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"
     ```
     ```
     Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"
     ```
1. También puede que necesite configurar la Directiva de grupo siguiente: 
    * **Configuración del equipo** \> **plantillas administrativas** \> la **delegación de credenciales** \> **del sistema** \> **permitir delegar credenciales nuevas con autenticación de servidor solo NTLM**
    * Haga clic en **Habilitar** y agregue *wsman/FQDN-of-Hyper-v-host*.
1. Abra **el administrador de Hyper-V**.
1. En el panel izquierdo, haga clic con el botón secundario en **Administrador de Hyper-V**.
1. Haga clic en **conectar al servidor**.

>[!NOTE]
> Esto solo funcionará para hosts **remotos** de windows Server 2016 o Windows 10.

Para obtener información sobre los cmdlets, consulte [set-Item](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/set-item) y [enable-WSManCredSSP](https://technet.microsoft.com/library/hh849872.aspx).

## <a name="install-hyper-v-manager"></a>Instalar el administrador de Hyper-V

Para usar una herramienta de interfaz de usuario, elija la que sea adecuada para el sistema operativo en el equipo en el que va a ejecutar el administrador de Hyper-V:

En Windows Server, abra Administrador del servidor \> **administrar** \> **Agregar roles y características**. Vaya a la página **características** y expanda **herramientas de administración remota del servidor** \> herramientas de administración de **roles** \> **herramientas de administración de Hyper-V**. 

En Windows, el administrador de Hyper-V está disponible en [cualquier sistema operativo de Windows que incluya Hyper-v](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility).

1. En el escritorio de Windows, haga clic en el botón Inicio y comience a escribir **programas y características**. 
1. En resultados de la búsqueda, haga clic en **programas y características**.
1. En el panel izquierdo, haga clic en **activar o desactivar las características de Windows**.
1. Expanda la carpeta Hyper-V y **haga clic en herramientas de administración de Hyper-v**.
1. Para instalar el administrador de Hyper-V, haga clic en **herramientas de administración de Hyper-v**. Si también desea instalar el módulo de Hyper-V, haga clic en esa opción.

Para usar Windows PowerShell, ejecute el siguiente comando como administrador:

```
add-windowsfeature rsat-hyper-v-tools
```

## <a name="see-also"></a>Consulte también  
 
[Instalar Hyper-V](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md) 

