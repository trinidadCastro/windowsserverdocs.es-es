---
title: netcfg
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4895928ffdd5d923d370f82e699d69f42c0f81a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838938"
---
# <a name="netcfg"></a>netcfg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|/c|Proporciona la **clase** del componente que se va a instalar; Protocolo, servicio o cliente|
|/i|Proporciona el **identificador** del componente|
|/s|Proporciona el tipo de componentes que se van a **Mostrar**.<p>\ta = adaptadores, n = componentes de net|
|/b|Muestra las **rutas**de acceso de enlace, cuando van seguidas de una cadena que contiene el nombre de la ruta de acceso.|
|/?|Muestra la **ayuda** en el símbolo del sistema.|

## <a name="examples"></a><a name=BKMK_Examples></a>Example

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
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
