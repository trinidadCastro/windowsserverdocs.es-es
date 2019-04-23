---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Recopilar información de red
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d88cc2fafd9aa4fc221efc901c48fa41ef007a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867046"
---
# <a name="collecting-network-information"></a>Recopilar información de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Es el primer paso para diseñar una topología de sitio efectiva en los servicios de dominio de Active Directory (AD DS) que puede consultar el grupo de red de su organización para recopilar información y comunicarse con ellos con regularidad acerca de la topología de red física.  
  
## <a name="creating-a-location-map"></a>Creación de un mapa de ubicación

Cree un mapa de ubicación que representa la infraestructura de red física de su organización. En el mapa de ubicación, identificar las ubicaciones geográficas que contienen grupos de equipos con conectividad interna de 10 megabits por segundo (Mbps) o superior (velocidad de red de área local (LAN) o superior).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Lista de vínculos de comunicación y el ancho de banda disponible

Después de crear un mapa de ubicación, el tipo de vínculo de comunicación, la velocidad de vínculo y el ancho de banda disponible entre cada ubicación de documento. Obtenga una topología de área extensa (WAN) de la red de su grupo de red. Para obtener una lista de tipos comunes de circuito WAN y los anchos de banda, consulte la sección "Cómo determinar el costo" en [crear un diseño de vínculo de sitio](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Necesitará esta información para crear vínculos a sitios más adelante en el proceso de diseño de topología de sitio.  
  
Ancho de banda hace referencia a la cantidad de datos que pueden transmitir a través de un canal de comunicación en un período de tiempo determinado. Ancho de banda disponible se refiere a la cantidad de ancho de banda disponible para su uso con AD DS. Puede obtener información de ancho de banda disponible en el grupo de red o puede analizar el tráfico en cada vínculo mediante el uso de un analizador de protocolos como Monitor de red. Para obtener información acerca de cómo instalar el Monitor de red, consulte el artículo [supervisa el tráfico de red](https://go.microsoft.com/fwlink/?LinkId=107058).  
  
Documente cada ubicación y otras ubicaciones que están vinculadas a ella. Además, registre el tipo de vínculo de comunicación y su ancho de banda disponible. Para que una hoja de cálculo para ayudarle en la lista de vínculos de comunicación y el ancho de banda disponible, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, y Abra "Geográfica ubicaciones y vínculos de comunicación" (DSSTOPO_1.doc).  
  
## <a name="listing-ip-subnets-within-each-location"></a>Lista de subredes IP dentro de cada ubicación

Después de documentar los vínculos de comunicación y el ancho de banda disponible entre cada ubicación, registre las subredes IP de cada ubicación. Si aún no conoce la máscara de subred y la dirección IP dentro de cada ubicación, consulte el grupo de red.  
  
AD DS asocia una estación de trabajo a un sitio comparando la dirección IP de la estación de trabajo con las subredes que están asociados con cada sitio. A medida que agrega controladores de dominio a un dominio, AD DS también examina sus direcciones IP y los coloca en el sitio más adecuado.  
  
Para que una hoja de cálculo para ayudarle en el listado de las subredes IP de cada ubicación, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abra " Ubicaciones y subredes"(DSSTOPO_2.doc).  
  
> [!NOTE]  
> Además de IP versión 4 (IPv4) direcciones, Windows Server es compatible con IP versión 6 (IPv6) también los prefijos de subred. Para que una hoja de cálculo para ayudarle en el listado de los prefijos de subred IPv6, consulte [Apéndice A: Ubicaciones y prefijos de subred](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>Lista de dominios y número de usuarios para cada ubicación

El número de usuarios para cada dominio regional que se representa en una ubicación es uno de los factores que determinan la colocación de los controladores de dominio regional y los servidores de catálogo global, que es el siguiente paso en el proceso de diseño de topología de sitio. Por ejemplo, planea colocar un controlador de dominio regional en una ubicación que contiene más de 100 usuarios del dominio regional, por lo que pueden seguir iniciar sesión en el dominio si se produce un error en el vínculo WAN.  
  
Registre las ubicaciones, los dominios que se representan en cada ubicación y el número de usuarios para cada dominio que se representa en cada ubicación. Para que una hoja de cálculo que le ayudarán a enumerar los dominios y el número de usuarios que se representan en cada ubicación, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_ Security_Services.zip y abrir "dominios y los usuarios en cada ubicación" (DSSTOPO_3.doc).  
