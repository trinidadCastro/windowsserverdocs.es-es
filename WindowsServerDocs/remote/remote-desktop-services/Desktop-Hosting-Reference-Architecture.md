---
title: Arquitectura de referencia de hospedaje de escritorio
description: Guía de arquitectura para crear una solución de hospedaje de escritorio con RDS y Azure.
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
ms.openlocfilehash: 01560a3758963c17c4ea0cb94b806c3b99193464
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "63749238"
---
# <a name="desktop-hosting-reference-architecture"></a>Arquitectura de referencia de hospedaje de escritorio

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

En este artículo se define un conjunto de bloques arquitectónicos para utilizar Máquinas virtuales de Servicios de Escritorio remoto y de Microsoft Azure con el fin de crear servicios de escritorio y de aplicación multiinquilinos y hospedados en Windows, lo cual se denomina en este documento "hospedaje de escritorio". Puedes usar esta referencia de arquitectura para crear soluciones de hospedaje de escritorio muy seguras, escalables y confiables para pequeñas y medianas organizaciones de entre 5 y 5000 usuarios.    
  
La primera audiencia de esta arquitectura de referencia son los proveedores de hospedaje que desean aprovechar los servicios de infraestructura de Microsoft Azure para proporcionar servicios de hospedaje de escritorio y licencias de acceso de suscriptor (SAL) a varios inquilinos a través del programa del [Contrato de licencias del proveedor de servicios (SPLA)](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) de Microsoft. La segunda audiencia de esta arquitectura de referencia son los clientes finales que desean crear y administrar soluciones de hospedaje de escritorio en los servicios de infraestructura de Microsoft Azure para sus propios empleados mediante [licencias CAL de usuario de RDS con derechos extendidos a través de Software Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).   
  
Para proporcionar soluciones de hospedaje de escritorio, los asociados de hospedaje y los clientes de SA aprovechan Windows Server para proporcionar a los usuarios de Windows una experiencia de aplicación que resulta conocida para los usuarios profesionales y los consumidores. Creado basándose en los aspectos básicos de Windows 10, Windows Server 2016 proporciona una compatibilidad con aplicaciones y una experiencia de usuario conocidas.    
  
El ámbito de este documento se limita a:   
  
* Una guía de diseño de arquitectura para un servicio de hospedaje de escritorio. La información detallada como los procedimientos de implementación, el rendimiento y el planeamiento de capacidad se explica en documentos independientes. Para más información acerca de los servicios de infraestructura de Azure, consulta [Microsoft Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).   
  
* Los escritorios basados en sesiones, las aplicaciones de RemoteApp y los escritorios personales basados en servidores que usan el host de sesión de Escritorio remoto de Windows Server 2016. No se tratan aquí las infraestructuras de escritorio virtual basadas en clientes ya que no hay ningún contrato de licencia de proveedor de servicios (SPLA) para los sistemas operativos cliente de Windows. Las infraestructuras de escritorio virtual basadas en Windows Server se permiten en los contratos de licencia de proveedor de servicios y las basadas en clientes de Windows se permiten en hardware dedicado con licencias de cliente final en determinados escenarios. No obstante, las infraestructuras de escritorio virtual basadas en clientes están fuera del ámbito de este documento.   
  
* Los productos y características de Microsoft, principalmente a Windows Server 2016 y a los servicios de infraestructura de Microsoft Azure.   
  
* Servicios de hospedaje de escritorio para inquilinos que incluyen entre 5 y 5000 usuarios.   Para inquilinos más grandes, es posible que necesites modificar esta arquitectura para ofrecer un rendimiento adecuado. La interfaz gráfica de usuario de RDS del Administrador del servidor no es recomendable para implementaciones de más de 500 usuarios. Se recomienda PowerShell para administrar implementaciones de RDS entre 500 y 5000 usuarios.   
  
* El conjunto mínimo de componentes y servicios necesarios para un servicio de hospedaje de escritorio. Hay muchos componentes y servicios opcionales que se pueden agregar para mejorar un servicio de hospedaje de escritorio, pero éstos están fuera del ámbito de este documento.    
  
Después de leer este documento, el lector debe conocer:   
- Los bloques de creación necesarios para proporcionar una solución de hospedaje de escritorio segura, confiable y multiinquilino basada en los servicios de Microsoft Azure.  
- El propósito de cada bloque de creación y cómo encajan entre sí.  
  
Hay varias formas de crear una solución de hospedaje de escritorio basada en esta arquitectura. Esta arquitectura describe las mejoras y la integración de Azure con Windows Server 2016. Hay otras opciones de implementación disponibles con la [Guía de arquitectura de referencia de hospedaje de escritorio](https://go.microsoft.com/fwlink/p/?LinkId=517389) para Windows Server 2012 R2.    
  
Se tratan los temas siguientes:  
- [Arquitectura lógica de hospedaje de escritorio](Desktop-hosting-logical-architecture.md)  
- [Descripción de los roles de RDS](Understanding-RDS-roles.md)
- [Descripción del entorno de hospedaje de escritorio](Understanding-the-desktop-hosting-environment.md)  
- [Servicios de Azure y consideraciones para el hospedaje de escritorio](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


