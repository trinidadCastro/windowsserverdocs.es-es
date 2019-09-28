---
title: Herramientas de línea de comandos del Administrador de recursos del servidor de archivos
description: En este artículo se describen las herramientas de línea de comandos de Windows Server 2016
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 78c054c5b0c3de19d1f3acd825335eab2f140541
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394298"
---
# <a name="file-server-resource-manager-command-line-tools"></a>Herramientas de línea de comandos del Administrador de recursos del servidor de archivos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

El Administrador de recursos del servidor de archivos instala los cmdlets de PowerShell [FileServerResourceManager](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager), así como las siguientes herramientas de línea de comandos:

-   **Dirquota.exe**. Se usa para crear y administrar cuotas, cuotas automáticas y plantillas de cuota.
-   **Filescrn.exe**. Se usa para crear y administrar filtros de archivos, plantillas de filtro de archivos, excepciones de filtro de archivos y grupos de archivos.
-   **Storrept.exe**. Se usa para configurar los parámetros de informe y generar informes de almacenamiento a petición. También se usa para crear tareas de informe, que puedes programar usando **schtasks.exe**.

Puedes usar estas herramientas para administrar recursos de almacenamiento en un equipo local o en un equipo remoto. Para obtener más información acerca de estas herramientas de línea de comandos, consulta las siguientes referencias:

-   **Dirquota**: <https://go.microsoft.com/fwlink/?LinkId=92741>
-   **Filescrn**: <https://go.microsoft.com/fwlink/?LinkId=92742>
-   **Storrept**: <https://go.microsoft.com/fwlink/?LinkId=92743>


> [!Note]
> Para ver la sintaxis de comandos y los parámetros disponibles de un comando, ejecuta el comando con el parámetro <strong>/?</strong> .


## <a name="remote-management-using-the-command-line-tools"></a>Administración remota con las herramientas de línea de comandos

Cada herramienta tiene varias opciones para realizar acciones similares a las que están disponibles en el complemento MMC del Administrador de recursos del servidor de archivos. Para que un comando realice una acción en un equipo remoto en lugar de realizarla en el equipo local, usa el parámetro **/remote**:*ComputerName*.

Por ejemplo,**Dirquota.exe** incluye un parámetro **template export** para escribir la configuración de plantilla de cuota en un archivo XML y un parámetro **template import** para importar la configuración de plantilla desde el archivo XML. Agregar el parámetro **/remote**:*ComputerName* al comando **Dirquota.exe template import** importará las plantillas desde el archivo XML del equipo local al equipo remoto.

> [!Note]
> Si ejecutas las herramientas de línea de comandos con el parámetro **/remote**:<em>ComputerName</em> para realizar una exportación (o importación) de plantilla en un equipo remoto, las plantillas se escribirán en un archivo XML, o se copiarán desde el mismo, del equipo local.

<br />

## <a name="additional-considerations"></a>Consideraciones adicionales 

Para administrar recursos remotos con las herramientas de línea de comandos:

-   Debes iniciar sesión con una cuenta de dominio que forme parte del grupo de **Administradores** del equipo local y del equipo remoto.
-   Debes ejecutar las herramientas de línea de comandos desde una ventana del símbolo del sistema con privilegios elevados. Para abrir la ventana del símbolo del sistema con privilegios elevados, haga clic en **Inicio**, seleccione **Todos los programas**, **Accesorios**, haga clic con el botón secundario en **Símbolo del sistema** y, a continuación, haga clic en **Ejecutar como administrador**.
-   El equipo remoto debe ejecutar Windows Server y debe tener instalado el Administrador de recursos del servidor de archivos.
-   La excepción **Administración del Administrador de recursos del servidor de archivos remoto** del equipo remoto debe estar habilitada. Habilita esta excepción usando el Firewall de Windows en el Panel de control.


## <a name="see-also"></a>Vea también

-   [Administrar recursos de almacenamiento remoto](managing-remote-storage-resources.md)