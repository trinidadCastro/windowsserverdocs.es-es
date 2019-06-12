---
title: Preparar el entorno para Windows Admin Center
description: Preparar el entorno para Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d018ea65ce61cab67fe2041b9ef885d32de51b17
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811916"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>Preparar el entorno para Windows Admin Center

> Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Hay algunas versiones de servidor que necesitan preparación adicional antes de que estén preparadas para administrarse con Windows Admin Center:

- [Windows Server 2012 and 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server 2008 R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

## <a name="prepare-windows-server-2012-and-2012-r2"></a>Preparar Windows Server 2012 y 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>Instalar WMF versión 5.1 o posterior

Windows Admin Center requiere características de PowerShell que no se incluyen de forma predeterminada en Windows Server 2012 y 2012 R2. Para administrar Windows Server 2012 o 2012 R2 con Windows Admin Center, debes instalar WMF versión 5.1 o posterior en esos servidores.

Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior.

Si no está instalado, puedes [descargar e instalar WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

## <a name="prepare-windows-server-2008-r2"></a>Preparar Windows Server 2008 R2

### <a name="install-wmf-version-51-or-higher"></a>Instalar WMF versión 5.1 o posterior

Windows Admin Center requiere características de PowerShell que no se incluyen de forma predeterminada en Windows Server 2008 R2. Para administrar Windows Server 2008 R2 con Windows Admin Center, debes instalar WMF versión 5.1 o posterior en esos servidores. 

Asegúrese de que [.NET Framework 4.5.2 o posterior](https://docs.microsoft.com/dotnet/framework/install/on-windows-7) ya está instalado en el equipo.

Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior.

Si no está instalado, puedes [descargar e instalar WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

Ejecuta `Enable-PSRemoting –force` en una consola de PowerShell para habilitar la conexión remota de Powershell. 

### <a name="enable-remote-desktop"></a>Habilitar Escritorio remoto

Para utilizar Escritorio remoto dentro de Windows Admin Center, debes habilitar Escritorio remoto en tu servidor de Windows Server 2008 R2.

Desde el **Administrador de servidores**, ve a **Configurar Escritorio remoto**. Habilita Escritorio remoto en "Permitir las conexiones desde equipos que ejecuten cualquier versión de Escritorio remoto".

## <a name="prepare-microsoft-hyper-v-server-2016"></a>Preparar Microsoft Hyper-V Server 2016

Para administrar Microsoft Hyper-V Server 2016 con Windows Admin Center, hay algunos roles de servidor que necesitarás para habilitar antes de poder hacerlo.

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>Para administrar Microsoft Hyper-V Server 2016 con Windows Admin Center:

1. Habilita Administración remota.
2. Habilita el rol de servidor de archivos.
3. Habilita el módulo de Hyper-V para PowerShell.

### <a name="step-1-enable-remote-management"></a>**Paso 1:** Habilitar la administración remota

Para habilitar la administración remota en Hyper-V Server:

1. Inicia sesión en Hyper-V Server.
2. En la herramienta **Configuración del servidor** (SCONFIG), escribe **4** para configurar la administración remota.
3. Escribe **1** para habilitar la administración remota.
4. Escribe **4** para volver al menú principal.

### <a name="step-2-enable-file-server-role"></a>**Paso 2:** Habilitar el rol de servidor de archivos

Para habilitar el rol de servidor de archivos para el uso compartido básico de archivos y la administración remota:

1. Haz clic en **Roles y características** en el menú **Herramientas** .
2. En **Roles y características**, busca **Servicios de archivos y almacenamiento** y marca**Servicios de iSCSI y archivo** y **Servidor de archivos**:

![Captura de pantalla de Roles y características que se muestra el archivo y seleccionarlo el rol de servicios de iSCSI](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**Paso 3:** Habilitar el módulo de Hyper-V para PowerShell

Para habilitar el módulo de Hyper-V para las características de PowerShell:

1. Haz clic en **Roles y características** en el menú **Herramientas** .
2. En **Roles y características**, busca **Herramientas de administración remota del servidor** y marca **Herramientas de administración de roles** y **Módulo de Hyper-V para PowerShell**:

![Captura de pantalla de Roles y características que muestra las funciones de Hyper-V seleccionado](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 ahora está listo para la administración con Windows Admin Center.

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>Preparar Microsoft Hyper-V Server 2012 R2

Para administrar Microsoft Hyper-V Server 2012 R2 con Windows Admin Center, hay algunos roles de servidor que tendrás que habilitar antes de poder hacerlo.  Además, debes instalar WMF versión 5.1 o posterior.

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>Para administrar Microsoft Hyper-V Server 2012 R2 con Windows Admin Center:

1. Instalar Windows Management Framework (WMF) versión 5.1 o posterior
2. Habilitar la administración remota
3. Habilitar el rol de servidor de archivos
4. Habilitar el módulo de Hyper-V para PowerShell

### <a name="step-1-install-windows-management-framework-51"></a>Paso 1: Instale Windows Management Framework 5.1

Windows Admin Center requiere características de PowerShell que no se incluyen de forma predeterminada en Microsoft Hyper-V Server 2012 R2. Para administrar Microsoft Hyper-V Server 2012 R2 con Windows Admin Center, debes instalar WMF versión 5.1 o posterior.

Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior. 

Si no está instalado, puedes [descargar WMF 5.1](https://docs.microsoft.com/powershell/wmf/5.1/install-configure).

### <a name="step-2-enable-remote-management"></a>Paso 2: Habilitar la administración remota

Para habilitar la administración remota de Hyper-V Server:

1. Inicia sesión en Hyper-V Server.
2. En la herramienta **Configuración del servidor** (SCONFIG), escribe **4** para configurar la administración remota.
3. Escribe **1** para habilitar la administración remota.
4. Escribe **4** para volver al menú principal.

### <a name="step-3-enable-file-server-role"></a>Paso 3: Habilitar el rol de servidor de archivos

Para habilitar el rol de servidor de archivos para el uso compartido básico de archivos y la administración remota:

1. Haz clic en **Roles y características** en el menú **Herramientas** .
2. En **Roles y características**, busca **Servicios de archivos y almacenamiento** y marca **Servicios de iSCSI y archivo** y **Servidor de archivos**:

![Captura de pantalla de Roles y características que se muestra el archivo y seleccionarlo el rol de servicios de iSCSI](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>Paso 4: Habilitar el módulo de Hyper-V para PowerShell

Para habilitar el módulo de Hyper-V para las características de PowerShell:

1. Haz clic en **Roles y características** en el menú **Herramientas** .
2. En **Roles y características**, busca **Herramientas de administración remota del servidor** y marca **Herramientas de administración de roles** y **Módulo de Hyper-V para PowerShell**:

![Captura de pantalla de Roles y características con herramientas de administración remota del servidor de Hyper-V seleccionadas](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 ahora está listo para la administración con Windows Admin Center.

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)