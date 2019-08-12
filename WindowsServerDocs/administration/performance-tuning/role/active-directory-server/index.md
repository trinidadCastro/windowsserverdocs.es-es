---
title: Optimización del rendimiento para servidores de Active Directory
description: Optimización del rendimiento para servidores de Active Directory
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab; v-tea
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: b8ab1eed003294e3396bcea21b31b7c084180b9c
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863447"
---
# <a name="performance-tuning-active-directory-servers"></a>Optimización del rendimiento para servidores de Active Directory

La optimización del rendimiento de Active Directory se centra en dos objetivos:
- Active Directory se configure de forma óptima para atender la carga de la manera más eficiente posible
- Las cargas de trabajo enviadas a Active Directory deben ser tan eficaces como sea posible

Esto requiere prestar atención adecuada a tres áreas diferentes:
- Adecuado planeamiento de la capacidad: para garantizar que haya hardware suficiente para admitir la carga existente
- Optimización del servidor: configurar controladores de dominio para administrar la carga de la forma más eficiente posible
- Optimización del cliente/aplicación de Active Directory: para garantiza que los clientes y las aplicaciones usen Active Directory de manera óptima

## <a name="start-with-capacity-planning"></a>Comenzar con el planeamiento de la capacidad

Implementar correctamente un número suficiente de controladores de dominio, en el dominio adecuado, en las configuraciones regionales correctas, y para dar cabida a la redundancia es esencial para garantizar atender las solicitudes de los clientes de manera oportuna. Se trata de un tema profundo que escapa al ámbito de esta guía. Se anima a los lectores a leer y entender las recomendaciones y guías que se encuentran en [Planeamiento de la capacidad para Active Directory Domain Services](capacity-planning-for-active-directory-domain-services.md) para comenzar la optimización del rendimiento de su instancia de Active Directory.

>[!Important]
> La configuración y el tamaño correctos de Active Directory tienen una repercusión considerable en el rendimiento general del sistema y de las cargas de trabajo. Se recomienda encarecidamente a los lectores a que lean primero [Capacity Planning for Active Directory Domain Services](capacity-planning-for-active-directory-domain-services.md).

## <a name="updates-and-evolving-recommendations"></a>Actualizaciones y recomendaciones en evolución

Durante las últimas varias generaciones del sistema operativo han tenido lugar grandes mejoras en las optimizaciones del rendimiento de clientes y de Active Directory, y estos esfuerzos se mantienen. Se recomienda implementar las versiones más recientes de la plataforma para obtener las ventajas, entre otras:

- Una mayor confiabilidad
- Un mejor rendimiento
- Un mejor registro y herramientas para solucionar problemas

Sin embargo, sabemos que esto requiere tiempo, y muchos entornos se ejecutan en un escenario donde es imposible la adopción del 100 % de la plataforma más reciente. Se han agregado algunas mejoras a las versiones anteriores de la plataforma, y seguiremos agregando más.

Para mantenerse al día con las noticias, las instrucciones y los procedimientos recomendados más recientes para administrar AD DS, te recomendamos que sigas el blog de nuestro equipo ["Ask the Directory Services Team"](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/bg-p/AskDS) (Pregunta al equipo de servicios de directorio).

## <a name="see-also"></a>Consulte también

- [Planeamiento de capacidad para AD DS](capacity-planning-for-active-directory-domain-services.md)
- [Consideraciones de hardware](hardware-considerations.md).
- [Consideraciones de uso de memoria](memory-usage-considerations.md)
- [Consideraciones de LDAP](ldap-considerations.md)
- [Colocación adecuada de los controladores de dominio y consideraciones de sitio](site-definition-considerations.md)
- [Solución de problemas de rendimiento de AD DS](troubleshoot.md)  
  
