---
title: 'Cliente de escritorio remoto: configuración admitida'
description: Obtenga información sobre qué equipos se puede tener acceso mediante los clientes de escritorio remoto
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884696"
---
# <a name="remote-desktop-client---supported-configuration"></a>Cliente de escritorio remoto: configuración admitida

## <a name="supported-pcs"></a>Equipos compatibles
Puede conectarse a equipos que ejecutan los siguientes sistemas operativos de Windows:
- Windows 10 Pro
- Windows 10 Enterprise
- Windows 8 Enterprise
- Windows 8 Professional
- Windows 7 Professional
- Windows 7 Enterprise
- Windows 7 Ultimate
- Windows 7 Ultimate
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

Los siguientes equipos pueden ejecutar la puerta de enlace de escritorio remoto:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

Los siguientes sistemas operativos puede actuar como servidores de acceso Web de escritorio remoto o RemoteApp:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Las ediciones y versiones de Windows no admitido

El cliente de escritorio remoto no se conectará a estas ediciones y versiones de Windows:

- Windows 7 Starter
- Windows 7 Home
- Windows 8 Home
- Página principal de Windows 8.1
- Windows 10 Home

Si desea tener acceso a equipos que tengan una de estas versiones de Windows instaladas, se recomienda que actualice a una versión de Windows que es compatible con RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>No se admite la mensajería de puerta de enlace de escritorio remoto
Cliente de escritorio remoto no es compatible con la mensajería de puerta de enlace de escritorio remoto. Compruebe que el escritorio recurso directiva de acceso remoto (RAP de RD) para el servidor de puerta de enlace de escritorio remoto no especifica **Permitir solo los equipos con el soporte técnico para la mensajería de puerta de enlace de escritorio remoto** o no podrá conectarse.