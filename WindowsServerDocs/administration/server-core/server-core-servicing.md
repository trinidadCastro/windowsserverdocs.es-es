---
title: Aplicación de revisiones de Server Core
description: Obtenga información sobre cómo actualizar una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: b19512a6f34e13469433aba6051f1232824beb0e
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034157"
---
# <a name="patch-a-server-core-installation"></a>Revisión de una instalación Server Core

> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Puede aplicar revisiones a un servidor que ejecuta la instalación Server Core de las maneras siguientes:

- **Mediante Windows Update automáticamente o con Windows Server Update Services (WSUS)** . Mediante Windows Update, ya sea automáticamente o con herramientas de línea de comandos o Windows Server Update Services (WSUS), puede atender los servidores que ejecutan una instalación Server Core.

- **Manualmente**. Incluso en las organizaciones que no usan Windows update o WSUS, puede aplicar manualmente las actualizaciones.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Ver las actualizaciones instaladas en el servidor Server Core
Antes de agregar una nueva actualización para Server Core, es una buena idea para ver qué actualizaciones se han instalado.

Para ver las actualizaciones mediante Windows PowerShell, ejecute **Get-Hotfix**.

Para ver las actualizaciones mediante la ejecución de un comando, ejecute **systeminfo.exe**. Puede haber un breve retraso mientras la herramienta inspecciona el sistema.

También puede ejecutar **wmic qfe lista** desde la línea de comandos. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Revisión de Server Core automáticamente con Windows Update

Para revisar el servidor de forma automática con Windows Update, siga estos pasos:

1. Comprobar la configuración actual de Windows Update:
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

Si el servidor es miembro de un dominio, también puedes configurar Windows Update mediante directivas de grupo. Para obtener más información, consulta https://go.microsoft.com/fwlink/?LinkId=192470. Sin embargo, cuando se usa este método, solo la opción 4 ("Descargar automáticamente y programar la instalación") es relevante para las instalaciones Server Core debido a la falta de una interfaz gráfica. Para tener un mayor control sobre qué actualizaciones están instaladas y dónde, puede usar un script que funciona como el equivalente en línea de comandos para la mayor parte de la interfaz gráfica de Windows Update. Para obtener información acerca de la secuencia de comandos, consulte https://go.microsoft.com/fwlink/?LinkId=192471.

Para obligar a Windows Update a detectar e instalar inmediatamente cualquier actualización disponible, ejecute el siguiente comando:

```
Wuauclt /detectnow 
```

En función de las actualizaciones que estén instaladas, es posible que deba reiniciar el equipo, aunque el sistema no se lo notificará. Para determinar si se ha completado el proceso de instalación, use el Administrador de tareas para comprobar que la **Wuauclt** o **instalador de confianza** procesos no se están ejecutando. También puede usar los métodos de [ver las actualizaciones instaladas en el servidor Server Core](#view-the-updates-installed-on-your-server-core-server) para comprobar la lista de actualizaciones instaladas.

## <a name="patch-the-server-with-wsus"></a>Revisión del servidor con WSUS 

Si el servidor Server Core es miembro de un dominio, puede configurarlo para que use un servidor WSUS con directivas de grupo. Para obtener más información, descargue el [información de referencia de directiva de grupo](https://www.microsoft.com/download/details.aspx?id=25250). También puede revisar [configurar configuración de directiva de grupo para actualizaciones automáticas](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>Patch manualmente el servidor

Descargue la actualización y esté disponible para la instalación Server Core.
En un símbolo del sistema, ejecute el siguiente comando:

```
Wusa <update>.msu /quiet 
```

En función de las actualizaciones que estén instaladas, es posible que deba reiniciar el equipo, aunque el sistema no se lo notificará.

Para desinstalar manualmente una actualización, ejecute el siguiente comando:

```
Wusa /uninstall <update>.msu /quiet 
```

