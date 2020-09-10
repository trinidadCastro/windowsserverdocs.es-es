---
title: netcfg
description: Artículo de referencia del comando netcfg, que instala el Entorno de preinstalación de Windows (WinPE), una versión ligera de Windows que se usa para implementar estaciones de trabajo.
ms.topic: reference
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8680e2e4265f36058f8f0d7c57be144633060bba
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640337"
---
# <a name="netcfg"></a>netcfg

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Instala el Entorno de preinstalación de Windows (WinPE), una versión ligera de Windows que se usa para implementar estaciones de trabajo.

## <a name="syntax"></a>Sintaxis

```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /v | Se ejecuta en modo detallado (detallado). |
| /e | Usa variables de entorno de mantenimiento durante la instalación y desinstalación. |
| /winpe | Instala TCP/IP, NetBIOS y el cliente de Microsoft para el entorno de preinstalación de Windows (WinPE). |
| /l | Proporciona la ubicación del archivo INF. |
| /C | Proporciona la clase del componente que se va a instalar; **Protocolo**, **servicio**o **cliente**. |
| /i | Proporciona el identificador del componente. |
| /s | Proporciona el tipo de componentes que se van a mostrar, incluido **\ta** para los adaptadores o **n** para los componentes de net. |
| /b | Muestra las rutas de acceso de enlace, cuando van seguidas de una cadena que contiene el nombre de la ruta de acceso. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para instalar el *ejemplo* de protocolo con c:\oemdir\example.inf, escriba:

```
netcfg /l c:\oemdir\example.inf /c p /i example
```

Para instalar el servicio *MS_Server* , escriba:

```
netcfg /c s /i MS_Server
```

Para instalar TCP/IP, NetBIOS y el cliente de Microsoft para el entorno de preinstalación de Windows, escriba:

```
netcfg /v /winpe
```

Para mostrar si el componente *MS_IPX* está instalado, escriba:

```
netcfg /q MS_IPX
```

Para desinstalar *MS_IPX*de componentes, escriba:

```
netcfg /u MS_IPX
```

Para mostrar todos los componentes de red instalados, escriba:

```
netcfg /s n
```

Para mostrar las rutas de acceso de enlace que contienen *MS_TCPIP*, escriba:

```
netcfg /b ms_tcpip
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
