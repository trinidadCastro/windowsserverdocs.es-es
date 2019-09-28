---
title: 'Servicios de Escritorio remoto: Multi-Factor Authentication'
description: Información de planificación para usar MFA con RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 833eafd0b762098b67b11e6e5f26f63e62057fd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403845"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Servicios de Escritorio remoto: Multi-Factor Authentication

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016

Aprovecha la eficacia de Active Directory con Multi-Factor Authentication para aplicar la protección de alta seguridad de tus recursos empresariales.

Cuando los usuarios finales se conecten a sus escritorios y aplicaciones, la experiencia que obtendrán será similar a la que ya usan al realizar una segunda medida autenticación para conectarse al recurso deseado:
- Iniciar un escritorio o RemoteApp desde un archivo RDP o a través de una aplicación cliente del Escritorio remoto.
- Al conectarse a la puerta de enlace del Escritorio remoto para obtener un acceso seguro y remoto, los usuarios recibirán un SMS o un desafío de MFA para aplicaciones móviles.
- Se autenticarán correctamente y se podrán conectar al recurso.

Para obtener más detalles sobre el proceso de configuración, consulta [Integrate your Remote Desktop Gateway infrastructure using the Network Policy Server (NPS) extension and Azure AD](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway) (Integra tu infraestructura de la Puerta de enlace de Escritorio remoto con la extensión del Servidor de directivas de red (NPS) y Azure AD).
