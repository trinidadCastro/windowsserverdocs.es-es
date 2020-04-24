---
title: Uso del Liberador de espacio en disco en Windows Server
description: Obtén información acerca de cómo usar las opciones de línea de comandos para configurar la herramienta Liberador de espacio en disco (cleanmgr.exe) para limpiar automáticamente determinados archivos.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: bb93ec15fd138ee65797c9d27413552c3a1759a6
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "75949671"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Uso del Liberador de espacio en disco en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2

La herramienta Liberador de espacio en disco borra los archivos innecesarios en un entorno de Windows Server. Esta herramienta está disponible de forma predeterminada en Windows Server 2019 y Windows Server 2016, pero podrían ser necesarios algunos pasos manuales para habilitarla en versiones anteriores de Windows Server.

Para iniciar la herramienta Liberador de espacio en disco, ejecuta el comando Cleanmgr.exe o selecciona **Inicio**, **Herramientas administrativas de Windows** y, a continuación, **Liberador de espacio en disco**.

También puedes abrir el Liberador de espacio en disco mediante el [comando de Windows cleanmgr](../../administration/windows-commands/cleanmgr.md), así como usar las opciones de línea de comandos para indicar al Liberador de espacio en disco que limpie determinados archivos.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Habilitación del Liberador de espacio en disco en una versión anterior de Windows Server mediante la instalación de la Experiencia de escritorio

Sigue estos pasos para usar el Asistente para agregar roles y características en la instalación de la Experiencia de escritorio en un servidor que ejecuta Windows Server 2012 R2 o una versión anterior. También se instalará el Liberador de espacio en disco.

1. Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.

   - En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.

   - Ve a **Inicio** y selecciona el icono Administrador del servidor.

1. En el menú **Administrar**, haz clic en agregar **Roles y características**.

1. En la página **Antes de comenzar**, comprueba que el servidor de destino y el entorno de red estén preparados para la característica que quieres instalar. Selecciona **Siguiente**.

1. En la página **Seleccionar tipo de instalación**, selecciona **Instalación basada en características o en roles** para instalar todos los elementos en un mismo servidor. Selecciona **Siguiente**.

1. En la página **Seleccionar servidor de destino** , seleccione un servidor del grupo de servidores o un VHD sin conexión. Selecciona **Siguiente**.

1. En la pantalla **Seleccionar roles de servidor**, selecciona **Siguiente**.

1. En la página **Seleccionar características**, selecciona **User Interface and Infrastructure** (Infraestructura e interfaz de usuario) y, a continuación, selecciona **Experiencia de escritorio**.

1. En **¿Desea agregar características requeridas para servidor DHCP?** , haz clic en **Agregar características**.

1. Continúa con la instalación y, a continuación, reinicia el sistema.

1. Comprueba que el botón de opción **Liberador de espacio en disco** aparece en el cuadro de diálogo Propiedades.

   ![Diálogo de propiedades del disco con la opción Liberador de espacio en disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Adición manual del Liberador de espacio en disco a una versión anterior de Windows Server

La herramienta Liberador de espacio en disco (cleanmgr.exe) no está presente en Windows Server 2012 R2 ni en versiones anteriores, a menos que tengas instalada la característica Experiencia de escritorio.

Para usar cleanmgr.exe, instala la Experiencia de escritorio como se describió anteriormente, o bien copia dos archivos que ya estén presentes en el servidor: cleanmgr.exe y cleanmgr.exe.mui. Usa la tabla siguiente para buscar los archivos correspondientes a tu sistema operativo.

| Sistema operativo  | Arquitectura  | Ubicación del archivo  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Busca cleanmgr.exe y mueve el archivo a **%systemroot%\System32**.

Busca cleanmgr.exe.mui y mueve los archivos a **%systemroot%\System32\en-US**.

Ahora puedes iniciar la herramienta Liberador de espacio en disco al ejecutar Cleanmgr.exe desde el símbolo del sistema o al hacer clic en **Inicio** y escribir **Cleanmgr** en la barra de búsqueda.

Para que aparezca el botón Liberador de espacio en disco en el cuadro de diálogo Propiedades de un disco, también deberás instalar la característica Experiencia de escritorio.

## <a name="additional-references"></a>Referencias adicionales

[Liberar espacio en la unidad en Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
