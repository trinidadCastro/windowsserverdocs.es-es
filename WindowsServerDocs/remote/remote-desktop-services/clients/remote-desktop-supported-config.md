---
title: Cliente de escritorio remoto - configuración compatible
description: Obtenga información sobre el equipo en el que puede tener acceso a través de los clientes de escritorio remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d38008b6387385917ad21ce7e169b8ff3f4d18ba
ms.sourcegitcommit: 96e968bbe8dc50ebb1535ae1c8ce92fa73c83171
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "1978052"
---
# <a name="remote-desktop-client---supported-configuration"></a>Cliente de escritorio remoto - configuración compatible

## <a name="supported-pcs"></a>Equipos compatibles
Puede conectarse a los equipos que ejecutan los siguientes sistemas operativos de Windows:
- Windows10 Pro
- Windows 10 Enterprise
- Windows 8 Enterprise
- Windows 8 Professional
- Windows 7 Professional
- Windows 7 Enterprise
- Windows 7 Ultimate
- Windows 7 Ultimate
- WindowsServer2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server2012R2
- WindowsServer2016
- Windows Server multipunto de 2011
- Windows Server multipunto de 2012
- Windows Small Business Server 2008
- Windows Small Business Server2011

Los siguientes equipos pueden ejecutar la puerta de enlace de escritorio remoto:

- WindowsServer2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server2012R2
- WindowsServer2016
- Windows Small Business Server2011

Los siguientes sistemas operativos pueden actuar como servidores de acceso Web de escritorio remoto o RemoteApp:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server2012R2
- WindowsServer2016

## <a name="unsupported-windows-versions-and-editions"></a>Las ediciones y versiones de Windows no compatible

El cliente de escritorio remoto no se conectará a estas versiones de Windows y las ediciones:

- Windows 7 Starter
- Página principal de Windows 7
- Página principal de Windows 8
- Página principal de Windows 8.1
- Windows 10 Home

Si desea tener acceso a los equipos que tienen una de estas versiones de Windows instaladas, se recomienda que actualizar a una versión de Windows que es compatible con RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>No se admite la mensajería de puerta de enlace de escritorio remoto
Cliente de escritorio remoto no es compatible con la mensajería de puerta de enlace de escritorio remoto. Compruebe que el escritorio recurso de directiva de acceso remoto (RAP de RD) para el servidor de puerta de enlace de escritorio remoto no especificar **sólo permitir que los equipos con compatibilidad para la mensajería de puerta de enlace de escritorio remoto** o no podrán conectarse.