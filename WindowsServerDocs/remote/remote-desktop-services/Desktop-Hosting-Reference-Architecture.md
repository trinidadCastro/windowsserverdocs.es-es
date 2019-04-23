---
title: Arquitectura de referencia de hospedaje de escritorio
description: Guía de arquitectura para crear una solución con RDS y Azure de hospedaje de escritorio.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: 6f235fd89c34c00601c802f4ea71e440af630169
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890246"
---
# <a name="desktop-hosting-reference-architecture"></a>Arquitectura de referencia de hospedaje de escritorio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este artículo define un conjunto de bloques arquitectónicos para usar servicios de escritorio remoto (RDS) y las máquinas virtuales de Microsoft Azure para crear varios inquilinos, hospedadas en el escritorio de Windows y de aplicación Servicios, lo que llamamos "hospedaje de escritorio." Puede usar esta referencia de arquitectura para crear soluciones para pequeñas y medianas organizaciones con usuarios de 5 a 5000 de hospedaje de escritorio altamente seguro, escalable y confiable.    
  
Los destinatarios principales de esta arquitectura de referencia son proveedores de hospedaje que desean aprovechar los servicios de infraestructura de Microsoft Azure para ofrecer servicios de hospedaje de escritorio y licencias de acceso de suscriptor (sal) a varios inquilinos a través de la [ Contrato de licencia de proveedor de servicios de Microsoft](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) programa (SPLA). Un segundo público para esta arquitectura de referencia son los clientes finales que desean crear y administrar soluciones de hospedaje de escritorio en servicios de infraestructura de Microsoft Azure para sus propios empleados mediante [derechos a través de Software ampliados de CAL de usuario de RDS Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf) (SA).   
  
Para entregar un escritorio hospedar soluciones, hospedaje de los socios y clientes de SA aprovechan Windows Server para ofrecer una experiencia de aplicación que está familiarizada con los consumidores y los usuarios empresariales de los usuarios de Windows. Se basa en los fundamentos de Windows 10, Windows Server 2016 proporciona compatibilidad con aplicaciones familiares y el usuario experimente.    
  
El ámbito de este documento se limita a:   
  
* Guía de diseño de arquitectura para un servicio de hospedaje de escritorio. Información detallada, como los procedimientos de implementación, el rendimiento y el planeamiento de capacidad se explica en documentos independientes. Para obtener información general acerca de los servicios de infraestructura de Azure, consulte [Microsoft Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).   
  
* Escritorios personales basada en servidor que usan Windows Server 2016 remoto escritorio (RD Session Host), las aplicaciones de RemoteApp y escritorios basados en sesión. No están cubiertas Windows basada en cliente infraestructuras de escritorio virtual porque no hay ningún contrato de licencia de proveedor de servicios (SPLA) para los sistemas operativos de cliente de Windows. Infraestructuras de escritorio virtual basado en servidor de Windows se permiten en lo contratos SPLA, y Windows basada en cliente infraestructuras de escritorio virtual se permiten en un hardware dedicado con licencias de cliente final en determinados escenarios. Sin embargo, infraestructuras de escritorio virtual basado en cliente están fuera del ámbito de este documento.   
  
* Productos de Microsoft y las características, principalmente, Windows Server 2016 y servicios de infraestructura de Microsoft Azure.   
  
* Hospedaje de servicios para los inquilinos que dispongan de 5 a 5.000 usuarios de escritorio.   Para los inquilinos más grandes, es posible que necesite modificar esta arquitectura para ofrecer un rendimiento adecuado. La interfaz gráfica de usuario de administrador del servidor RDS (GUI) no se recomienda para las implementaciones de más de 500 usuarios. Se recomienda PowerShell para administrar implementaciones de RDS entre 500 y 5000 usuarios.   
  
* El conjunto mínimo de componentes y servicios necesarios para un servicio de hospedaje de escritorio. Hay muchos componentes opcionales y los servicios que se pueden agregar para mejorar un servicio de hospedaje de escritorio, pero éstos están fuera del ámbito de este documento.    
  
Después de leer este documento, el lector debe conocer:   
- Los bloques de creación necesarios proporcionar un solución basada en servicios de Microsoft Azure de hospedaje de escritorio seguro, confiable y para varios inquilinos.  
- El propósito de cada bloque de creación y cómo encajan entre sí.  
  
Hay varias formas de crear una solución basada en esta arquitectura de hospedaje de escritorio. Esta arquitectura describe las mejoras en Azure con Windows Server 2016 e integración. Otras opciones de implementación están disponibles con el [Guía de arquitectura de referencia de hospedaje de escritorio](https://go.microsoft.com/fwlink/p/?LinkId=517389) para Windows Server 2012 R2.    
  
Se tratan los temas siguientes:  
- [Arquitectura lógica hospedaje de escritorio](Desktop-hosting-logical-architecture.md)  
- [Entender los Roles RDS](Understanding-RDS-roles.md)
- [Comprender el entorno de hospedaje de escritorio](Understanding-the-desktop-hosting-environment.md)  
- [Servicios de Azure y consideraciones para el hospedaje de escritorio](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


