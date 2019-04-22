---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Crear un diseño de sitio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d896213708f8c3ec5de44a1f85fb4ebd86b8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819076"
---
# <a name="creating-a-site-design"></a>Crear un diseño de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crear un diseño de sitio implica decidir qué ubicaciones se convertirán en sitios, crear objetos de sitio, crear objetos de subred y asociar las subredes a sitios.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidir qué ubicaciones se convertirán en sitios

Decida qué ubicaciones para crear sitios para los siguientes:  
  
- Crear sitios para todas las ubicaciones en el que va a colocar los controladores de dominio. Consulte la información documentada en la hoja de cálculo de "Ubicación del controlador de dominio" (DSSTOPO_4.doc) para identificar las ubicaciones que incluyen controladores de dominio.  
- Crear sitios para esas ubicaciones que incluyan servidores que ejecutan aplicaciones que requieren que se puede crear un sitio. Determinadas aplicaciones, como archivo de sistema distribuido espacios de nombres (DFSN), utilizan objetos de sitio para buscar los servidores más cercanos a los clientes.  

   > [!NOTE]  
   > Si su organización tiene varias redes de cerca con conexiones rápidas y confiables, puede incluir todas las subredes para estas redes en un único sitio de Active Directory. Por ejemplo, si la devolución de ida y vuelta de red latencia entre dos servidores en diferentes subredes es de 10 ms o menos, puede incluir ambas subredes en el mismo sitio de Active Directory. Si la latencia de red entre las dos ubicaciones es mayor que 10 ms, no debe incluir las subredes en un único sitio de Active Directory. Incluso cuando la latencia es de 10 ms o menos, puede optar por implementar sitios independientes si desea segmentar el tráfico entre sitios para las aplicaciones basadas en Active Directory.  

- Si no es necesario para una ubicación de un sitio, agregue la subred de la ubicación en un sitio para el que la ubicación tiene la velocidad de máximo de área extensa (WAN) de red y el ancho de banda disponible.  
  
Ubicaciones de documentos que se convertirá en sitios y las direcciones de red y máscaras de subred dentro de cada ubicación. Para que una hoja de cálculo que le ayudarán a documentar sitios, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abra "asociar subredes con Sitios"(DSSTOPO_6.doc).  
  
## <a name="creating-a-site-object-design"></a>Crear un diseño de objeto de sitio

Para cada ubicación donde ha decidido crear sitios, planee crear objetos de sitio en servicios de dominio de Active Directory (AD DS). Ubicaciones de documento que se convertirán en sitios en la hoja de cálculo "Asociar subredes con los sitios".  
  
Para obtener más información sobre cómo crear objetos de sitio, consulte el artículo [crear un sitio](https://go.microsoft.com/fwlink/?LinkId=107067).  
  
## <a name="creating-a-subnet-object-design"></a>Crear un diseño de objeto de subred

Para cada subred IP y máscara de subred asociada con cada ubicación, planee crear objetos de subred en AD DS que representa todas las direcciones IP dentro del sitio.  
  
Al crear un objeto de subred de Active Directory, la información acerca de la subred IP de red y máscara de subred se convierte automáticamente en el formato de notación de longitud de prefijo de red <IP address> / <prefix length>. Por ejemplo, la red IP versión 4 (IPv4) dirección 172.16.4.0 con una máscara de subred 255.255.252.0 aparece como 172.16.4.0/22. Además de las direcciones IPv4, Windows Server 2008 también es compatible con IP versión 6 (IPv6) subred prefijos, por ejemplo, 3FFE:FFFF:0:C000:: / 64. Para obtener más información acerca de las subredes IP en cada ubicación, consulte la hoja de cálculo "Ubicaciones y subredes" (DSSTOPO_2.doc) [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) y [Apéndice A: Ubicaciones y prefijos de subred](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Asociar cada objeto de subred con un objeto de sitio mediante una referencia a la hoja de cálculo "Asociar subredes con los sitios" (DSSTOPO_6.doc) en la sección de "Decidir que ubicaciones le se convierten en los sitios" para determinar qué subred está asociarse con un sitio determinado. Documento que el objeto de subred de Active Directory asociado a cada ubicación en la hoja de cálculo "Asociar subredes con los sitios" (DSSTOPO_6.doc).  
  
Para obtener más información sobre cómo crear objetos de subred, consulte el artículo [crear una subred](https://go.microsoft.com/fwlink/?LinkId=107068).
