---
title: Mediante el Liberador de espacio en disco en Windows Server
description: Obtenga información sobre cómo usar las opciones de línea de comandos para configurar la herramienta de limpieza de disco (Cleanmgr.exe) para limpiar automáticamente determinados archivos.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: b479697366239144e5ca9d3486b84191eb51dc4d
ms.sourcegitcommit: 078304c4b92bb57eb85ba29634afc92cc028c644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67301617"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Mediante el Liberador de espacio en disco en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La herramienta de limpieza de disco borra los archivos innecesarios en un entorno de Windows Server. Esta herramienta está disponible de forma predeterminada en Windows Server 2019 y Windows Server 2016, pero es posible que deba realizar algunos pasos manuales para habilitarlo en versiones anteriores de Windows Server.

Para iniciar el Liberador de espacio en disco, ejecute el comando Cleanmgr.exe o seleccione **iniciar**, seleccione **herramientas administrativas de Windows**y, a continuación, seleccione **Liberador de espacio en disco**.

También puede ejecutar el Liberador de espacio en disco mediante el uso de la [cleanmgr comando Windows](../../administration/windows-commands/clean-mgr.md) y usar las opciones de línea de comandos para especificar que determinados archivos limpia Liberador de espacio en disco.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Habilitar el Liberador de espacio en disco en una versión anterior de Windows Server mediante la instalación de la experiencia de escritorio

Siga estos pasos para usar el Asistente de las características y agregar Roles para instalar la experiencia de escritorio en un servidor que ejecuta Windows Server 2012 R2 o versiones anteriores, lo que también instala Liberador de espacio en disco.

1. Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.

   - En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.

   - Vaya a **iniciar** y seleccione el icono de administrador del servidor.

1. En el **administrar** menú, seleccione Agregar **Roles y características**.

1. En el **antes de comenzar** , comprueba que el servidor de destino y el entorno de red estén preparados para la característica que desea instalar. Selecciona **Siguiente**.

1. En el **Seleccionar tipo de instalación** , seleccione **instalación basada en roles o basada en características** para instalar todas las características de los elementos en un único servidor. Selecciona **Siguiente**.

1. En la página **Seleccionar servidor de destino**, seleccione un servidor del grupo de servidores o un VHD sin conexión. Selecciona **Siguiente**.

1. En el **seleccionar roles de servidor** , seleccione **siguiente**.

1. En el **seleccionar características** , seleccione **interfaz de usuario y la infraestructura**y, a continuación, seleccione **experiencia de escritorio**.

1. En **agregar características requeridas para la experiencia de escritorio?** , seleccione **agregar características**.

1. Continuar con la instalación y, a continuación, reinicie el sistema.

1. Compruebe que la **Liberador de espacio en disco** botón de opción aparece en el cuadro de diálogo de propiedades.

   ![Cuadro de diálogo de propiedades con la opción de liberar espacio en disco del disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Agregar manualmente el Liberador de espacio en disco a una versión anterior de Windows Server

La herramienta de limpieza de disco (cleanmgr.exe) no está presente en Windows Server 2012 R2 o versiones anteriores a menos que tenga instalada la característica experiencia de escritorio.

Para usar cleanmgr.exe, instalar la experiencia de escritorio, como se describió anteriormente, o copiar dos archivos que ya están presentes en el servidor, cleanmgr.exe y cleanmgr.exe.mui. Use la tabla siguiente para localizar los archivos para su sistema operativo.

| Sistema operativo  | Arquitectura  | Ubicación del archivo  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Busque cleanmgr.exe y mover el archivo **%systemroot%\System32**.

Busque cleanmgr.exe.mui y mover los archivos **%systemroot%\System32\en-US**.

Ahora puede iniciar la herramienta de limpieza de disco ejecutando Cleanmgr.exe desde el símbolo del sistema o haciendo clic en **iniciar** y escribiendo **Cleanmgr** en la barra de búsqueda.

Para que el botón de liberar espacio en disco aparezca en el cuadro de diálogo de propiedades de un disco, también deberá instalar la característica experiencia de escritorio.

## <a name="additional-references"></a>Referencias adicionales

[Libere espacio en disco en Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/clean-mgr.md)
