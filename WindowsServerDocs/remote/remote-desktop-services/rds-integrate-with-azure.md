---
title: 'Servicios de Escritorio remoto: integración con servicios de Azure'
description: Aprende a integrar Servicios de Escritorio remoto en tu implementación de Azure, y a Azure en tu implementación de RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/18/2017
ms.topic: article
author: lizap
ms.openlocfilehash: 2c792a22608fe0e7ae826b27d6917202037559e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855078"
---
# <a name="remote-desktop-services---integrating-with-azure-services"></a>Servicios de Escritorio remoto: integración con servicios de Azure

Windows Server 2016 combina la eficaz entrega segura de escritorios y aplicaciones mediante Servicios de Escritorio remoto con los servicios flexibles y escalables proporcionados por Microsoft Azure. Puedes implementar RDS con servicios Azure para ayudar a reducir el costo de mantenimiento de la infraestructura de los servidores locales, aumentar la estabilidad mediante el uso de los servicios Azure para garantizar una alta disponibilidad, mejorar la seguridad mediante el uso de la autenticación multifactor y mejorar la experiencia de los usuarios mediante el uso de las identidades existentes para acceder a los recursos en RDS.

Usa la siguiente información para integrar Azure en la implementación de Escritorio remoto:

- [Obtén información sobre cómo usar autenticación multifactor con RDS](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
- [Integrar Azure AD Domain Services con la implementación de RDS](rds-azure-adds.md)
- [Publicar el Escritorio remoto con el proxy de aplicación de Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop)

Para ver cómo estos servicios simplifican la arquitectura de la implementación del Escritorio remoto, consulta la sección [Arquitecturas RDS con funciones exclusivas de PaaS de Azure](desktop-hosting-logical-architecture.md#rds-architectures-with-unique-azure-paas-roles).