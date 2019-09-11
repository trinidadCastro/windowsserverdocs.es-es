---
title: Uso del liberador de espacio en disco en Windows Server
description: Obtenga información acerca de cómo usar las opciones de línea de comandos para configurar la herramienta liberador de espacio en disco (cleanmgr. exe) para limpiar automáticamente determinados archivos.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 4bf32520dc6fa2be36d44fbd66a7efc885a8f5d7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867409"
---
# <a name="using-disk-cleanup-on-windows-server"></a>Uso del liberador de espacio en disco en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

La herramienta liberador de espacio en disco borra los archivos innecesarios en un entorno de Windows Server. Esta herramienta está disponible de forma predeterminada en Windows Server 2019 y Windows Server 2016, pero es posible que tenga que realizar algunos pasos manuales para habilitarlo en versiones anteriores de Windows Server.

Para iniciar la herramienta liberador de espacio en disco, ejecute el comando cleanmgr. exe o seleccione **Inicio**, seleccione **herramientas administrativas de Windows**y, a continuación, seleccione **liberador de espacio en disco**.

También puede ejecutar el liberador de espacio en disco mediante el [comando de Windows cleanmgr](../../administration/windows-commands/cleanmgr.md) y usar las opciones de la línea de comandos para especificar que el liberador de espacio en disco Limpie determinados archivos.

## <a name="enable-disk-cleanup-on-an-earlier-version-of-windows-server-by-installing-the-desktop-experience"></a>Habilitar el liberador de espacio en disco en una versión anterior de Windows Server mediante la instalación de la experiencia de escritorio

Siga estos pasos para usar el Asistente para agregar roles y características para instalar la experiencia de escritorio en un servidor que ejecute Windows Server 2012 R2 o una versión anterior, que también instala el liberador de espacio en disco.

1. Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.

   - En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.

   - Vaya a **Inicio** y seleccione el icono de administrador del servidor.

1. En el menú **administrar** , seleccione Agregar **roles y características**.

1. En la página **antes de comenzar** , compruebe que el servidor de destino y el entorno de red estén preparados para la característica que desea instalar. Seleccione **Next** (Siguiente).

1. En la página **Seleccionar tipo de instalación** , seleccione Instalación basada en **características o en roles** para instalar todas las características de las partes en un solo servidor. Seleccione **Next** (Siguiente).

1. En la página **Seleccionar servidor de destino**, seleccione un servidor del grupo de servidores o un VHD sin conexión. Seleccione **Next** (Siguiente).

1. En la página **Seleccionar roles de servidor** , seleccione **siguiente**.

1. En la página **seleccionar características** , seleccione **infraestructura e interfaz de usuario**y, a continuación, seleccione **experiencia de escritorio**.

1. En **Agregar características que son necesarias para la experiencia de escritorio**, seleccione **Agregar características**.

1. Continúe con la instalación y, a continuación, reinicie el sistema.

1. Compruebe que el botón de opción **liberador de espacio en disco** aparece en el cuadro de diálogo Propiedades.

   ![Cuadro de diálogo Propiedades del disco con la opción de limpieza de disco](media/diskpropswcleanup.png)

## <a name="manually-add-disk-cleanup-to-an-earlier-version-of-windows-server"></a>Agregar manualmente el liberador de espacio en disco a una versión anterior de Windows Server

La herramienta liberador de espacio en disco (cleanmgr. exe) no está presente en Windows Server 2012 R2 o una versión anterior a menos que tenga instalada la característica experiencia de escritorio.

Para usar cleanmgr. exe, instale la experiencia de escritorio como se describió anteriormente, o bien copie dos archivos que ya estén presentes en el servidor, cleanmgr. exe y cleanmgr. exe. MUI. Use la tabla siguiente para buscar los archivos del sistema operativo.

| Sistema operativo  | Arquitectura  | Ubicación del archivo  |
| ----------------- | -------------- | --------------- |
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr_31bf3856ad364e35_6.1.7600.16385_none_c9392808773cd7da\cleanmgr.exe 
| Windows Server 2008 R2 | 64 bits | C:\Windows\winsxs\amd64_microsoft-windows-cleanmgr.resources_31bf3856ad364e35_6.1.7600.16385_en-us_b9cb6194b257cc63\cleanmgr.exe.mui |

Busque cleanmgr. exe y mueva el archivo a **%SystemRoot%\System32**.

Busque cleanmgr. exe. mui y mueva los archivos a **%systemroot%\System32\en-US**.

Ahora puede iniciar la herramienta liberador de espacio en disco ejecutando cleanmgr. exe desde el símbolo del sistema o haciendo clic en **Inicio** y escribiendo **cleanmgr** en la barra de búsqueda.

Para que aparezca el botón liberador de espacio en disco en el cuadro de diálogo Propiedades de un disco, también deberá instalar la característica experiencia de escritorio.

## <a name="additional-references"></a>Referencias adicionales

[Liberar espacio de la unidad en Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)

[cleanmgr](../../administration/windows-commands/cleanmgr.md)
