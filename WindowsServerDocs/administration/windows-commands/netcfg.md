---
title: netcfg
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aed535f843da6d735526ea97c07f94564dc00dc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871326"
---
# <a name="netcfg"></a>netcfg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala el entorno de preinstalación de Windows (WinPE), una versión ligera de Windows que se usa para implementar estaciones de trabajo.   
## <a name="syntax"></a>Sintaxis  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|/v|Ejecutar en el modo detallado (detallado)|  
|/e|Utilice el mantenimiento de las variables de entorno durante la instalación y desinstalación|  
|/WinPE|Instala TCP/IP, NetBIOS y el cliente de Microsoft para el entorno de preinstalación de Windows|  
|/l|Proporciona la ubicación de INF|  
|/c|Proporciona la clase del componente instalado; Protocolo, servicio o cliente|  
|/i|Proporciona el identificador de componente|  
|/s|Proporciona el tipo de componentes para mostrar<br /><br />\ta = adaptadores, n = los componentes de redes|  
|/?|Muestra la ayuda en el símbolo del sistema.|  
## <a name="BKMK_Examples"></a>Ejemplos  
Para instalar el protocolo *ejemplo* mediante c:\oemdir\example.inf:  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
Para instalar el *MS_Server* servicio:  
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
Para desinstalar el componente *MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
Para mostrar todos los componentes de redes instalados:  
```  
netcfg /s n  
```  
A las presentaciones que contiene las rutas de acceso de enlace *MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
