---
title: Arquitectura de Servicios de Escritorio remoto
description: Diagramas de arquitectura de RDS
ms.author: elizapo
ms.date: 02/10/2017
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: 756696dc4014801caf74a415117c205d59481ced
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955102"
---
# <a name="remote-desktop-services-architecture"></a>Arquitectura de Servicios de Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

A continuación, se muestran diversas configuraciones para la implementación de Servicios de Escritorio remoto para hospedar aplicaciones y escritorios de Windows para usuarios finales.

>[!NOTE]
> Los diagramas de arquitectura siguientes muestran el uso de RDS en Azure. No obstante, puedes implementar Servicios de Escritorio remoto en un entorno local y en otras nubes. Estos diagramas están pensados principalmente para ilustrar cómo se colocan los roles de RDS y cómo usan otros servicios.

## <a name="standard-rds-deployment-architectures"></a>Arquitecturas estándar de implementación de RDS

Servicios de Escritorio remoto incluye dos arquitecturas estándar:
-    Implementación básica: contiene el número mínimo de servidores para crear un entorno totalmente eficaz de RDS
-    Implementación de alta disponibilidad: contiene todos los componentes necesarios para proporcionar el mayor tiempo de actividad garantizado para el entorno de RDS

### <a name="basic-deployment"></a>Implementación básica

![Implementación básica de RDS](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Implementación de alta disponibilidad

![Implementación de alta disponibilidad de RDS](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Arquitecturas de RDS con roles exclusivos de PaaS de Azure

Aunque las arquitecturas estándar de implementación de RDS se adaptan a la mayoría de los escenarios, Azure sigue invirtiendo en soluciones de PaaS propias que generan valor para el cliente. A continuación, se muestran algunas arquitecturas que explican cómo incorporan los Servicios de Escritorio remoto.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Implementación de RDS con Azure AD Domain Services

Los dos diagramas de arquitecturas estándar anteriores se basan en una instancia tradicional de Active Directory (AD) implementada en una máquina virtual de Windows Server. Sin embargo, si no dispones de una instancia tradicional de AD y solo tienes un inquilino de Azure AD a través de servicios como Office 365, pero quieres sacar el máximo partido a los Servicios de Escritorio remoto, puedes usar [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) para crear un dominio totalmente administrado en el entorno de IaaS de Azure que utilice los mismos usuarios que ya existen en el inquilino de Azure AD. Esto acaba con la dificultad a la hora de sincronizar manualmente usuarios y de administrar más máquinas virtuales. Azure AD Domain Services puede funcionar con cualquier tipo de implementación: básica o de alta disponibilidad.

![Azure AD e implementación de RDS](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Implementación de RDS con Azure AD Application Proxy

Los dos diagramas de arquitectura estándar anteriores utilizan los servidores de acceso web y puerta de enlace de Escritorio remoto como punto de entrada desde Internet al sistema de RDS. En algunos entornos, los administradores prefieren quitar sus propios servidores del perímetro y, en su lugar, usar las tecnologías de proxy inverso que también proporcionan seguridad adicional. El rol de PaaS de [Azure AD Application Proxy](/azure/active-directory/active-directory-application-proxy-get-started) se adapta perfectamente a este escenario.

Para ver las configuraciones admitidas y cómo crear esta configuración, consulta cómo [publicar el Escritorio remoto con Azure AD Application Proxy](/azure/active-directory/application-proxy-publish-remote-desktop).

![RDS con Azure AD Application Proxy](./media/aadappproxy-rds.png)
