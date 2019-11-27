---
title: Implementación de Windows Server Update Services
description: 'Tema de Windows Server Update Services (WSUS): información general sobre el proceso de implementación con vínculos a los cuatro pasos necesarios para realizarlo'
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3e6bcd5f90d1a7df2a35dda45b4bf8951940815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361681"
---
# <a name="deploy-windows-server-update-services"></a>Implementación de Windows Server Update Services

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) permite a los administradores de tecnologías de la información implementar las actualizaciones de productos de Microsoft más recientes. WSUS es un rol de servidor de Windows Server que puede instalarse para administrar y distribuir actualizaciones. Un servidor WSUS puede ser el origen de actualización para otros servidores WSUS de la organización. El servidor WSUS que actúa como origen de actualización es el servidor que precede en la cadena.  

En una implementación de WSUS, al menos un servidor WSUS de la red debe conectarse a Microsoft Update para obtener información sobre actualizaciones disponibles. Puedes determinar, en función de la configuración y seguridad de red, la cantidad de servidores que quieres que se conecten directamente a Microsoft Update.  

En esta guía se ofrece información conceptual para planear e implementar Windows Server Update Services.  

-   [Planear la implementación de WSUS](../plan/plan-your-wsus-deployment.md)  

-   [Paso 1: Instalación del rol de servidor WSUS](1-install-the-wsus-server-role.md)  

-   [Paso 2: Configurar WSUS](2-configure-wsus.md)  

-   [Paso 3: Aprobar e implementar las actualizaciones en WSUS](3-approve-and-deploy-updates-in-wsus.md)  

-   [Paso 4: Configurar la directiva de grupo para las actualizaciones automáticas](4-configure-group-policy-settings-for-automatic-updates.md)  
