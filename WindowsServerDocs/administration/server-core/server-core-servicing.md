---
title: Revisión de Server Core
description: Obtener información sobre cómo actualizar una instalación Server Core de Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: eacb80d89e7bcc95d6b5c12269d7587dc7d6870c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383321"
---
# <a name="patch-a-server-core-installation"></a>Revisión de una instalación Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Puede aplicar una revisión a un servidor que ejecute la instalación Server Core de las siguientes maneras:

- **Usar Windows Update de forma automática o con Windows Server Update Services (WSUS)** . Mediante el uso de Windows Update, ya sea de forma automática o con herramientas de línea de comandos o Windows Server Update Services (WSUS), puede usar servidores de servicio que ejecuten una instalación Server Core.

- **Manualmente**. Incluso en organizaciones que no usan Windows Update o WSUS, puede aplicar las actualizaciones manualmente.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Ver las actualizaciones instaladas en el servidor Server Core
Antes de agregar una nueva actualización a Server Core, es una buena idea ver qué actualizaciones ya se han instalado.

Para ver las actualizaciones mediante Windows PowerShell, ejecute **Get-Hotfix**.

Para ver las actualizaciones mediante la ejecución de un comando, ejecute **SystemInfo. exe**. Puede haber un breve retraso mientras la herramienta inspecciona el sistema.

También puede ejecutar la **lista de WMIC QFE** desde la línea de comandos. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Patch Server Core automáticamente con Windows Update

Siga estos pasos para aplicar una revisión automática del servidor con Windows Update:

1. Compruebe la configuración de Windows Update actual:
   ```
   %systemroot%\system32\Cscript scregedit.wsf /AU /v 
   ```

2. Para habilitar las actualizaciones automáticas:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 4 
   Net start wuauserv
   ```  

3. Para deshabilitar las actualizaciones automáticas, ejecute:

   ```
   Net stop wuauserv 
   %systemroot%\system32\Cscript scregedit.wsf /AU 1 
   Net start wuauserv 
   ```

Si el servidor es miembro de un dominio, también puedes configurar Windows Update mediante directivas de grupo. Para obtener más información, consulta https://go.microsoft.com/fwlink/?LinkId=192470. Sin embargo, cuando se usa este método, solo la opción 4 ("descargar automáticamente y programar la instalación") es pertinente para las instalaciones Server Core debido a la falta de una interfaz gráfica. Para tener un mayor control sobre qué actualizaciones están instaladas y dónde, puede usar un script que funciona como el equivalente en línea de comandos para la mayor parte de la interfaz gráfica de Windows Update. Para obtener información sobre el script, vea https://go.microsoft.com/fwlink/?LinkId=192471.

Para obligar a Windows Update a detectar e instalar inmediatamente cualquier actualización disponible, ejecute el siguiente comando:

```
Wuauclt /detectnow 
```

En función de las actualizaciones que estén instaladas, es posible que deba reiniciar el equipo, aunque el sistema no se lo notificará. Para determinar si el proceso de instalación se ha completado, use el administrador de tareas para comprobar que los procesos de **wuauclt** o **instalador de confianza** no se están ejecutando de forma activa. También puede usar los métodos de para [ver las actualizaciones instaladas en el servidor Server Core](#view-the-updates-installed-on-your-server-core-server) para comprobar la lista de actualizaciones instaladas.

## <a name="patch-the-server-with-wsus"></a>Revisar el servidor con WSUS 

Si el servidor Server Core es miembro de un dominio, puede configurarlo para que use un servidor WSUS con directivas de grupo. Para obtener más información, descargue la [información de referencia de directiva de grupo](https://www.microsoft.com/download/details.aspx?id=25250). También puede revisar [configurar opciones de directiva de grupo para actualizaciones automáticas](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Revisar el servidor manualmente

Descargue la actualización y haga que esté disponible para la instalación Server Core.
En un símbolo del sistema, ejecute el siguiente comando:

```
Wusa <update>.msu /quiet 
```

En función de las actualizaciones que estén instaladas, es posible que deba reiniciar el equipo, aunque el sistema no se lo notificará.

Para desinstalar una actualización manualmente, ejecute el siguiente comando:

```
Wusa /uninstall <update>.msu /quiet 
```

