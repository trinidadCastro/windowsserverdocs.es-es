---
title: Aplicación de revisiones de Server Core
description: Obtenga información sobre cómo actualizar una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: f51ffae5ed8f91cca386eb209e7a1d8cc664ceeb
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1448208"
---
# <a name="patch-a-server-core-installation"></a>Revisión de una instalación Server Core

> Se aplica a: (delimitadas anuales del canal) de Windows Server y Windows Server 2016

Revisión de un servidor que ejecuta la instalación Server Core de las siguientes maneras:

- **Uso de Windows Update automáticamente o con Windows Server Update Services (WSUS)**. Mediante el uso de Windows Update, ya sea automáticamente o con las herramientas de línea de comandos o Windows Server Update Services (WSUS), que puede dar servicio los servidores que ejecutan una instalación Server Core.

- **Manualmente**. Incluso en las organizaciones que no usan Windows update o WSUS, puede aplicar manualmente las actualizaciones.

## <a name="view-the-updates-installed-on-your-server-core-server"></a>Ver las actualizaciones instaladas en el servidor de Server Core
Antes de agregar una nueva actualización de Server Core, es una buena idea para ver qué actualizaciones ya se han instalado.

Para ver las actualizaciones mediante el uso de Windows PowerShell, ejecute **Get-revisión**.

Para ver las actualizaciones mediante la ejecución de un comando, ejecute **systeminfo.exe**. Puede haber un breve retraso mientras la herramienta examina el sistema.

También puede ejecutar **wmic qfe lista** desde la línea de comandos. 

## <a name="patch-server-core-automatically-with-windows-update"></a>Revisión de Server Core automáticamente con Windows Update

Para revisar el servidor automáticamente con Windows Update, siga estos pasos:

1. Compruebe la configuración actual de Windows Update:
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

Si el servidor es un miembro de un dominio, también puede configurar Windows Update usando la directiva de grupo. Para obtener más información, consulte https://go.microsoft.com/fwlink/?LinkId=192470. Sin embargo, cuando se utiliza este método, única opción 4 ("Descargar automáticamente y programar la instalación") es relevante para las instalaciones Server Core debido a la falta de una interfaz gráfica. Para obtener más control sobre el que se instalan las actualizaciones y cuándo, puede usar una secuencia de comandos que proporciona un equivalente de línea de comandos de la mayoría de la interfaz gráfica de Windows Update. Para obtener información acerca de la secuencia de comandos, consulte https://go.microsoft.com/fwlink/?LinkId=192471.

Para forzar que Windows Update para detectar e instalar las actualizaciones disponibles de inmediatamente, ejecute el siguiente comando:

```
Wuauclt /detectnow 
```

Dependiendo de las actualizaciones que se instalan, debe reiniciar el equipo, aunque el sistema no le notificará de esto. Para determinar si se ha completado el proceso de instalación, use el Administrador de tareas para comprobar que no se están ejecutando activamente los procesos **Wuauclt** o **Instalador de confianza** . También puede utilizar los métodos en [vista de las actualizaciones instaladas en el servidor de Server Core](#view-the-updates-installed-on-your-Server-Core-server) para comprobar la lista de las actualizaciones instaladas.

## <a name="patch-the-server-with-wsus"></a>Revisión del servidor con WSUS 

Si el servidor de Server Core es un miembro de un dominio, puede configurar para que use un servidor WSUS con la directiva de grupo. Para obtener más información, descargue la [información de referencia de la directiva de grupo](https://www.microsoft.com/download/details.aspx?id=25250). Asimismo, puede revisar la [Configuración de la configuración de directiva de grupo para actualizaciones automáticas](../windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates.md)

## <a name="patch-the-server-manually"></a>El servidor de una revisión manualmente

Descargue la actualización y que esté disponible para la instalación Server Core.
En un símbolo del sistema, ejecute el siguiente comando:

```
Wusa <update>.msu /quiet 
```

Dependiendo de las actualizaciones que se instalan, debe reiniciar el equipo, aunque el sistema no le notificará de esto.

Para desinstalar una actualización manualmente, ejecute el siguiente comando:

```
Wusa /uninstall <update>.msu /quiet 
```

