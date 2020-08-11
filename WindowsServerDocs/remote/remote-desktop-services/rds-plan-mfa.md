---
title: 'Servicios de Escritorio remoto: Multi-Factor Authentication'
description: Información de planificación para usar MFA con RDS.
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 8d1edf1db673c70895e228a059bef753c617f617
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954972"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Servicios de Escritorio remoto: Multi-Factor Authentication

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Aprovecha la eficacia de Active Directory con Multi-Factor Authentication para aplicar la protección de alta seguridad de tus recursos empresariales.

Cuando los usuarios finales se conecten a sus escritorios y aplicaciones, la experiencia que obtendrán será similar a la que ya usan al realizar una segunda medida autenticación para conectarse al recurso deseado:
- Iniciar un escritorio o RemoteApp desde un archivo RDP o a través de una aplicación cliente del Escritorio remoto.
- Al conectarse a la puerta de enlace del Escritorio remoto para obtener un acceso seguro y remoto, los usuarios recibirán un SMS o un desafío de MFA para aplicaciones móviles.
- Se autenticarán correctamente y se podrán conectar al recurso.

Para obtener más detalles sobre el proceso de configuración, consulta [Integrate your Remote Desktop Gateway infrastructure using the Network Policy Server (NPS) extension and Azure AD](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway) (Integra tu infraestructura de la Puerta de enlace de Escritorio remoto con la extensión del Servidor de directivas de red (NPS) y Azure AD).
