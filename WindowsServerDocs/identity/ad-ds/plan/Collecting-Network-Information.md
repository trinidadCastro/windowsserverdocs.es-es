---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: "Recopilación de información de red"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 174f11ca85a659f9d0c52e220d5ce37b1804f39b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="collecting-network-information"></a>Recopilación de información de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Es el primer paso para diseñar una topología de sitios eficaz en los servicios de dominio de Active Directory (AD DS) consultar el grupo de red de su organización para recopilar información y comunicarse con ellos con regularidad sobre la topología de red física.  
  
## <a name="creating-a-location-map"></a>Creación de un mapa de ubicación  
Crear un mapa de la ubicación que representa la infraestructura de red física de la organización. En el mapa de la ubicación, identificar las ubicaciones geográficas que contienen los grupos de equipos con la conectividad interno de 10MB por segundo (Mbps) o superior (velocidad de red de área local (LAN) o superior).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Lista de vínculos de comunicación y ancho de banda disponible  
Después de crear un mapa de la ubicación, el tipo de enlace de comunicación, su velocidad del vínculo y el ancho de banda disponible entre cada ubicación de documento. Obtener una topología de área extensa (WAN) de la red de tu grupo de red. Para obtener una lista de tipos de circuito WAN comunes y sus anchos, consulta la sección "Determinar el costo" en [creación de un diseño de sitio vínculo](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Necesitarás esta información para crear vínculos a sitios más adelante en el proceso de diseño de la topología de sitio.  
  
Ancho de banda se refiere a la cantidad de datos que puede transmitir a través de un canal de comunicación en un determinado período de tiempo. Ancho de banda disponible hace referencia a la cantidad de ancho de banda disponible para su uso por AD DS. Puedes obtener información de ancho de banda disponible de tu grupo de red, o puede analizar el tráfico en cada vínculo mediante un analizador de protocolos como el Monitor de red, que es un componente que se incluye con Windows Server 2008. Para obtener información sobre cómo instalar el Monitor de red, vea Supervisar el tráfico de red ([https://go.microsoft.com/fwlink/?LinkId=107058](https://go.microsoft.com/fwlink/?LinkId=107058)).  
  
Documento de cada ubicación y otras ubicaciones que están vinculados a él. Además, registrar el tipo de enlace de comunicación y su ancho de banda disponible. Para que una hoja de cálculo para ayudarte en la lista de vínculos de comunicación y ancho de banda disponible, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, y Abre (DSSTOPO_1.doc) "Geográfica ubicaciones y vínculos de comunicación".  
  
## <a name="listing-ip-subnets-within-each-location"></a>Descripción de subredes IP dentro de cada ubicación  
Después de documentar los vínculos de comunicación y el ancho de banda disponible entre cada ubicación, registra las subredes IP dentro de cada ubicación. Si aún no conoce la máscara de subred y la dirección IP dentro de cada ubicación, consulte el grupo de red.  
  
AD DS asocia una estación de trabajo a un sitio comparando la dirección IP de la estación de trabajo con las subredes que están asociados a cada sitio. Agregar controladores de dominio a un dominio, AD DS también examina sus direcciones IP y los coloca en el sitio más apropiado.  
  
Para que una hoja de cálculo que le ayudarán a la descripción de las subredes IP dentro de cada ubicación, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, abre "ubicaciones y las subredes y los" (DSSTOPO_2.doc).  
  
> [!NOTE]  
> Además de IP versión 4 () las direcciones IPv4, Windows Server 2008 también admite IP versión 6 (IPv6) prefijos de subred. Para que una hoja de cálculo para ayudarte en el listado de los prefijos de subred IPv6, consulta [Apéndice A: ubicaciones y prefijos de subred](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
## <a name="listing-domains-and-number-of-users-for-each-location"></a>Lista de dominios y número de usuarios para cada ubicación  
El número de usuarios para cada dominio regional que se representa en una ubicación es uno de los factores que determinan la ubicación de los controladores de dominio regionales y servidores de catálogo global, que es el siguiente paso en el proceso de diseño de la topología de sitio. Por ejemplo, planea colocar un controlador de dominio regionales en una ubicación que contiene más de 100 usuarios del dominio regional por lo que pueden seguir iniciar sesión en el dominio si se produce un error en el vínculo WAN.  
  
Registra la ubicación, los dominios que se representan en cada ubicación y el número de usuarios para cada dominio que se representa en cada ubicación. Para que una hoja de cálculo que le ayudarán a la descripción de los dominios y el número de usuarios que se representan en cada ubicación, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar < DICT__Job_Aids_Designing_and_Deploying_ Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip > y abrir "Dominios y los usuarios en cada ubicación" (DSSTOPO_3.doc).  
  


