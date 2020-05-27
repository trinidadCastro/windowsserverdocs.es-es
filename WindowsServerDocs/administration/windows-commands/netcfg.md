---
title: netcfg
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c636b9a14d9dcee6f37409557ed2bcb87b1087d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820875"
---
# <a name="netcfg"></a>netcfg

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Instala el Entorno de preinstalación de Windows (WinPE), una versión ligera de Windows que se usa para implementar estaciones de trabajo.
## <a name="syntax"></a>Sintaxis
```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```
#### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/v|Ejecutar en modo **detallado** (detallado)|
|/e|Usar variables de **entorno** de mantenimiento durante la instalación y desinstalación|
|/winpe|Instala TCP/IP, NetBIOS y el cliente de Microsoft para el entorno de preinstalación de Windows (WinPE).|
|/l|Proporciona la **Ubicación** de INF|
|/C|Proporciona la **clase** del componente que se va a instalar; Protocolo, servicio o cliente|
|/i|Proporciona el **identificador** del componente|
|/s|Proporciona el tipo de componentes que se van a **Mostrar**.<p>\ta = adaptadores, n = componentes de net|
|/b|Muestra las **rutas**de acceso de enlace, cuando van seguidas de una cadena que contiene el nombre de la ruta de acceso.|
|/?|Muestra la **ayuda** en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para instalar el *ejemplo* de protocolo mediante c:\oemdir\example.inf:
```
netcfg /l c:\oemdir\example.inf /c p /i example
```
Para instalar el servicio *MS_Server* :
```
netcfg /c s /i MS_Server
```
Para instalar TCP/IP, NetBIOS y el cliente de Microsoft para el entorno de preinstalación de Windows
```
netcfg /v /winpe
```
Para mostrar si el componente *MS_IPX* está instalado:
```
netcfg /q MS_IPX
```
Para desinstalar *MS_IPX*de componentes:
```
netcfg /u MS_IPX
```
Para mostrar todos los componentes de red instalados:
```
netcfg /s n
```
Para mostrar las rutas de acceso de enlace que contienen *MS_TCPIP*:
```
netcfg /b ms_tcpip
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
