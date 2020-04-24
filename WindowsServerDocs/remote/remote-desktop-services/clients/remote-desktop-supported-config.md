---
title: 'Cliente de Escritorio remoto: configuración admitida'
description: Más información sobre qué equipos puedes acceder mediante los clientes de Escritorio remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 1480a2a14a1c3fc23c4e5122e366741d37d9091f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80856018"
---
# <a name="remote-desktop-client---supported-configuration"></a>Cliente de Escritorio remoto: configuración admitida

## <a name="supported-pcs"></a>Equipos compatibles
Puedes conectarte a equipos que ejecutan los siguientes sistemas operativos de Windows:
- Windows 10 Pro
- Windows 10 Enterprise
- Windows 8 Enterprise
- Windows 8 Professional
- Windows 7 Professional
- Windows 7 Enterprise
- Windows 7 Ultimate
- Windows 7 Ultimate
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

Los siguientes equipos pueden ejecutar la puerta de enlace de Escritorio remoto:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

Los siguientes sistemas operativos pueden actuar como servidores de acceso web de Escritorio remoto o de RemoteApp:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Ediciones y versiones de Windows no admitidas

El cliente de Escritorio remoto no se conectará a estas ediciones y versiones de Windows:

- Windows 7 Starter
- Windows 7 Home
- Windows 8 Home
- Windows 8.1 Home
- Windows 10 Home

Si quieres acceder a equipos que tienen alguna de estas versiones de Windows instaladas, te recomendamos que actualices a una versión de Windows que admita RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>No se admite la mensajería de puerta de enlace de Escritorio remoto
El cliente de Escritorio remoto no es compatible con la mensajería de puerta de enlace de Escritorio remoto. Comprueba que la directiva de acceso a recursos de Escritorio remoto (RD RAP) del servidor de puerta de enlace de Escritorio remoto no especifica la opción **Only allow computers with support for RD Gateway Messaging** (Permitir solo equipos con compatibilidad con la mensajería de puerta de enlace de Escritorio remoto).