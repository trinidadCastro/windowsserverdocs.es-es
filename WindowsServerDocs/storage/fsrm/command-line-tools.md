---
title: Herramientas de línea de comandos del servidor de archivos Administrador de recursos
description: En este artículo se describen las herramientas de línea de comandos de Windows Server 2016
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0300e8fa6845c58e61b695546daf4116286d975f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957522"
---
# <a name="file-server-resource-manager-command-line-tools"></a>Herramientas de línea de comandos del servidor de archivos Administrador de recursos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

El servidor de archivos Administrador de recursos instala los cmdlets de PowerShell de [FileServerResourceManager](/powershell/module/fileserverresourcemanager/?view=win10-ps) y las siguientes herramientas de línea de comandos:

-   **Dirquota.exe**. Se usa para crear y administrar cuotas, cuotas automáticas y plantillas de cuota.
-   **Filescrn.exe**. Se usa para crear y administrar filtros de archivos, plantillas de filtro de archivos, excepciones al filtro de archivos y grupos de archivos.
-   **Storrept.exe**. Se usa para configurar parámetros de informe y generar informes de almacenamiento a petición. También se usa para crear tareas de informes, que se pueden programar mediante **schtasks.exe**.

Puede usar estas herramientas para administrar recursos de almacenamiento en un equipo local o en un equipo remoto. Para obtener más información acerca de estas herramientas de línea de comandos, consulte las siguientes referencias:

-   **Dirquota**:<https://go.microsoft.com/fwlink/?LinkId=92741>
-   **Filescrn**:<https://go.microsoft.com/fwlink/?LinkId=92742>
-   **Storrept**:<https://go.microsoft.com/fwlink/?LinkId=92743>


> [!Note]
> Para ver la sintaxis de los comandos y los parámetros disponibles para un comando, ejecute el comando con el parámetro <strong>/?</strong> parámetro.


## <a name="remote-management-using-the-command-line-tools"></a>Administración remota con herramientas de línea de comandos

Cada herramienta tiene varias opciones para realizar acciones similares a las que se encuentran disponibles en el complemento MMC del Administrador de recursos del servidor de archivos. Para que un comando realice una acción en un equipo remoto en lugar de en el equipo local, use el parámetro **/Remote**:*ComputerName* .

Por ejemplo,**Dirquota.exe** incluye un parámetro de **exportación de plantilla** para escribir la configuración de la plantilla de cuota en un archivo XML y un parámetro de **importación de plantilla** para importar la configuración de la plantilla del archivo XML. Al agregar el parámetro **/Remote**:*ComputerName* al comando **Dirquota.exe template Import** se IMPORTArán las plantillas del archivo XML del equipo local en el equipo remoto.

> [!Note]
> Al ejecutar las herramientas de línea de comandos con el parámetro **/Remote**:<em>ComputerName</em> para realizar la exportación (o importación) de plantillas en un equipo remoto, las plantillas se escriben en un archivo XML del equipo local (o se copian desde él).

<br />

## <a name="additional-considerations"></a>Consideraciones adicionales

Para administrar recursos remotos con las herramientas de línea de comandos:

-   Debe haber iniciado sesión con una cuenta de dominio que sea miembro del grupo **administradores** en el equipo local y en el equipo remoto.
-   Debe ejecutar las herramientas de línea de comandos desde una ventana del símbolo del sistema con privilegios elevados. Para abrir la ventana del símbolo del sistema con privilegios elevados, haga clic en **Inicio**, seleccione **Todos los programas**, **Accesorios**, haga clic con el botón secundario en **Símbolo del sistema** y, a continuación, haga clic en **Ejecutar como administrador**.
-   El equipo remoto debe ejecutar Windows Server y el servidor de archivos Administrador de recursos debe estar instalado.
-   La excepción de **Administración de administrador de recursos del servidor de archivos remoto** en el equipo remoto debe estar habilitada. Habilite esta excepción mediante Firewall de Windows en el Panel de control.


## <a name="additional-references"></a>Referencias adicionales

-   [Administrar recursos de almacenamiento remoto](managing-remote-storage-resources.md)
