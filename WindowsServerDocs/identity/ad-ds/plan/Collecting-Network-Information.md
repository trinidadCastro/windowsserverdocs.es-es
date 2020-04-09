---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Recopilar información de red
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8f7df5bd57baee2ac8400bf1d9bc331426d65bf8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822828"
---
# <a name="collecting-network-information"></a>Recopilar información de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El primer paso para diseñar una topología de sitio efectiva en Active Directory Domain Services (AD DS) es consultar el grupo de redes de la organización para recopilar información y comunicarse con ellas periódicamente sobre la topología de red física.  
  
## <a name="creating-a-location-map"></a>Crear un mapa de ubicación

Cree un mapa de ubicación que represente la infraestructura de red física de su organización. En el mapa de ubicación, identifique las ubicaciones geográficas que contienen grupos de equipos con conectividad interna de 10 megabits por segundo (Mbps) o superior (velocidad de red de área local (LAN) o superior).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Enumerar los vínculos de comunicación y el ancho de banda disponible

Después de crear un mapa de ubicación, documente el tipo de vínculo de comunicación, su velocidad de vínculo y el ancho de banda disponible entre cada ubicación. Obtenga una topología de red de área extensa (WAN) del grupo de redes. Para obtener una lista de los tipos de circuito WAN comunes y sus anchos de banda, consulte la sección sobre cómo determinar el costo en [creación de un diseño de vínculo a sitios](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Necesitará esta información para crear vínculos a sitios más adelante en el proceso de diseño de la topología del sitio.  
  
El ancho de banda se refiere a la cantidad de datos que se pueden transmitir a través de un canal de comunicación en un período de tiempo determinado. El ancho de banda disponible se refiere a la cantidad de ancho de banda disponible realmente para su uso por AD DS. Puede obtener información de ancho de banda disponible en el grupo de red o puede analizar el tráfico en cada vínculo mediante un analizador de protocolos como Monitor de red. Para obtener información sobre la instalación de Monitor de red, consulte el artículo [supervisar el tráfico de red](https://go.microsoft.com/fwlink/?LinkId=107058).  
  
Documente cada ubicación y las demás ubicaciones que están vinculadas a ella. Además, registre el tipo de vínculo de comunicación y su ancho de banda disponible. Para ver una hoja de cálculo que le ayude a enumerar los vínculos de comunicación y el ancho de banda disponible, vea el tema [sobre ayudas de trabajo para el kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abrir "ubicaciones geográficas y vínculos de comunicación" (DSSTOPO_1. doc).  
  
## <a name="listing-ip-subnets-within-each-location"></a>Enumerar subredes IP dentro de cada ubicación

Después de documentar los vínculos de comunicación y el ancho de banda disponible entre cada ubicación, grabe las subredes IP dentro de cada ubicación. Si aún no conoce la máscara de subred y la dirección IP dentro de cada ubicación, consulte el grupo de redes.  
  
AD DS asocia una estación de trabajo a un sitio comparando la dirección IP de la estación de trabajo con las subredes asociadas a cada sitio. A medida que agrega controladores de dominio a un dominio, AD DS examina también sus direcciones IP y las coloca en el sitio más adecuado.  
  
Para que una hoja de cálculo le ayude a enumerar las subredes IP dentro de cada ubicación, vea el tema [sobre ayudas de trabajo para el kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abrir "ubicaciones y subredes" (DSSTOPO_2. doc).  
  
> [!NOTE]  
> Además de las direcciones IP versión 4 (IPv4), Windows Server también admite prefijos de subred de IP versión 6 (IPv6). Para ver una hoja de cálculo que le ayude a enumerar los prefijos de subred IPv6, consulte [apéndice a: ubicaciones y prefijos de subred](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>Enumerar los dominios y el número de usuarios de cada ubicación

El número de usuarios de cada dominio regional que se representa en una ubicación es uno de los factores que determinan la ubicación de los controladores de dominio regionales y los servidores de catálogo global, que es el paso siguiente en el proceso de diseño de la topología de sitio. Por ejemplo, piense en colocar un controlador de dominio regional en una ubicación que contenga más de 100 usuarios de dominio regional para que puedan iniciar sesión en el dominio si se produce un error en el vínculo WAN.  
  
Grabe las ubicaciones, los dominios que se representan en cada ubicación y el número de usuarios para cada dominio que se representa en cada ubicación. Para ver una hoja de cálculo que le ayude a enumerar los dominios y el número de usuarios que están representados en cada ubicación, vea el tema [sobre ayudas de trabajo para el kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abrir "dominios y usuarios en cada ubicación" (DSSTOPO_3. doc).  
