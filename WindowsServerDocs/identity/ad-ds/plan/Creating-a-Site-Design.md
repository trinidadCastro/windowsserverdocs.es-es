---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Crear un diseño de sitio
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c1b36a98cf7bbb7fed8c221a550f66b74c7df123
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624353"
---
# <a name="creating-a-site-design"></a>Crear un diseño de sitio

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La creación de un diseño de sitio implica la decisión de qué ubicaciones se convertirán en sitios, la creación de objetos de sitio, la creación de objetos de subred y la Asociación de subredes con sitios.

## <a name="deciding-which-locations-will-become-sites"></a>Decidir qué ubicaciones se convertirán en sitios

Decida en qué ubicaciones desea crear sitios de la siguiente manera:

- Cree sitios para todas las ubicaciones en las que planea colocar controladores de dominio. Consulte la información documentada en la hoja de cálculo "Ubicación del controlador de dominio" (DSSTOPO_4. doc) para identificar ubicaciones que incluyen controladores de dominio.
- Cree sitios para las ubicaciones que incluyen servidores que ejecutan aplicaciones que requieren la creación de un sitio. Ciertas aplicaciones, como Sistema de archivos distribuido espacios de nombres (DFSN), usan objetos de sitio para buscar los servidores más cercanos a los clientes.

> [!NOTE]
> Si su organización tiene varias redes en estrecha proximidad con conexiones rápidas y confiables, puede incluir todas las subredes de esas redes en un solo Active Directory sitio. Por ejemplo, si la latencia de red de retorno de ida y vuelta entre dos servidores de subredes diferentes es de 10 ms o menos, puede incluir ambas subredes en el mismo sitio Active Directory. Si la latencia de red entre las dos ubicaciones es superior a 10 ms, no debe incluir las subredes en un solo sitio Active Directory. Incluso cuando la latencia es de 10 ms o menos, puede optar por implementar sitios independientes si desea segmentar el tráfico entre sitios para aplicaciones basadas en Active Directory.

- Si no es necesario un sitio para una ubicación, agregue la subred de la ubicación a un sitio para el que la ubicación tenga la velocidad máxima de la red de área extensa (WAN) y el ancho de banda disponible.

Ubicaciones de documentos que se convertirán en sitios y las direcciones de red y máscaras de subred dentro de cada ubicación. Para obtener una hoja de cálculo que le ayude a documentar sitios, vea el tema [sobre ayudas de trabajo para el kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abrir "asociar subredes con sitios" (DSSTOPO_6. doc).

## <a name="creating-a-site-object-design"></a>Crear un diseño de objeto de sitio

Para cada ubicación en la que haya decidido crear sitios, planee la creación de objetos de sitio en Active Directory Domain Services (AD DS). Ubicaciones de documentos que se convertirán en sitios de la hoja de cálculo "asociar subredes con sitios".

Para obtener más información sobre cómo crear objetos de sitio, consulte el artículo [creación de un sitio](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772304(v=ws.11)).

## <a name="creating-a-subnet-object-design"></a>Crear un diseño de objeto de subred

Para cada subred IP y máscara de subred asociada a cada ubicación, planee crear objetos de subred en AD DS que representen todas las direcciones IP dentro del sitio.

Al crear un objeto de subred de Active Directory, la información acerca de la subred IP de red y la máscara de subred se traduce automáticamente <IP address> / <prefix length>en el formato de notación de longitud de prefijo de red. Por ejemplo, la dirección IP versión 4 (IPv4) de red 172.16.4.0 con una máscara de subred 255.255.252.0 aparece como 172.16.4.0/22. Además de las direcciones IPv4, Windows Server 2008 también admite prefijos de subred de IP versión 6 (IPv6), por ejemplo, 3FFE: FFFF: 0: c000::/64. Para obtener más información acerca de las subredes IP en cada ubicación, consulte la hoja de cálculo "ubicaciones y subredes" (DSSTOPO_2. doc) en [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) y [Apéndice A: ubicaciones y prefijos de subred](Appendix-A--Locations-and-Subnet-Prefixes.md).

Asocie cada objeto de subred a un objeto de sitio; para ello, consulte la sección "asociar subredes con sitios" (DSSTOPO_6. doc) en la sección "decidir qué ubicaciones se convertirán en sitios" para determinar qué subred se va a asociar a cada sitio. Documente el objeto de subred Active Directory asociado a cada ubicación en la hoja de cálculo "asociar subredes con sitios" (DSSTOPO_6. doc).

Para obtener más información sobre cómo crear objetos de subred, consulte el artículo [creación de una subred](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770372(v=ws.11)).
