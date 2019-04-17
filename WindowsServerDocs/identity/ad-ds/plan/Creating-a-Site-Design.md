---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: "Crear un diseño de sitio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2bcbf30159721e1fc2e12af103ca6f3e79f3ec97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-design"></a>Crear un diseño de sitio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crear un diseño de sitio implica decidir qué ubicaciones se convertirá en sitios, creación de objetos de sitio, creación de objetos de subred y asociar las subredes con los sitios.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidir qué ubicaciones se convertirá en sitios  
Decide qué ubicaciones para crear sitios para como sigue:  
  
-   Crear sitios para todas las ubicaciones en el que vas a colocar los controladores de dominio. Consulte la información incluida en la hoja de cálculo de (DSSTOPO_4.doc) "Ubicación del controlador de dominio" para identificar ubicaciones que incluyen controladores de dominio.  
  
-   Crear sitios para dichas ubicaciones que incluyen servidores que ejecutan aplicaciones que requieren un sitio a crearse. Ciertas aplicaciones, como archivos distribuido sistema espacios de nombres (DFSN), usan los objetos de sitios para localizar los servidores más cercanos a los clientes.  
  
    > [!NOTE]  
    > Si tu organización tiene varias redes cerca con conexiones rápidas y confiables, puedes incluir todas las subredes para esas redes en un único sitio de Active Directory. Por ejemplo, si la devolución de ida y vuelta de red latencia entre dos servidores en diferentes subredes es 10 ms o menos, puede incluir tanto en las subredes en el mismo sitio de Active Directory. Si la latencia de red entre las dos ubicaciones es superior a 10 ms, no debe incluir las subredes en un único sitio de Active Directory. Incluso cuando la latencia es 10 ms o menos, puede optar por implementar sitios independientes si quieres segmento el tráfico entre sitios para aplicaciones basadas en Active Directory.  
  
-   Si un sitio no es necesario para una ubicación, agrega la subred de la ubicación a un sitio para que la ubicación tiene la velocidad de máxima de área extensa (WAN) de la red y el ancho de banda disponible.  
  
Ubicaciones de documento que se convertirá en sitios y las direcciones de red y máscaras dentro de cada ubicación. Para que una hoja de cálculo que le ayudarán a documentar sitios, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y (DSSTOPO_6.doc) "Asociar subredes con sitios abiertos".  
  
## <a name="creating-a-site-object-design"></a>Creación de un diseño de objeto de sitio  
Para cada ubicación donde se ha decidido crear sitios, planea crear objetos de sitios en los servicios de dominio de Active Directory (AD DS). Ubicaciones de documento que se convertirá en sitios en la hoja de cálculo "Asociar subredes con sitios".  
  
Para obtener más información sobre cómo crear objetos de sitios, consulta crear un sitio ([https://go.microsoft.com/fwlink/?LinkId=107067](https://go.microsoft.com/fwlink/?LinkId=107067)).  
  
## <a name="creating-a-subnet-object-design"></a>Crear un diseño de objeto de subred  
Por cada subred IP y la máscara de subred asociada con cada ubicación, planea crear objetos de subred en AD DS que representa todas las direcciones IP dentro del sitio.  
  
Al crear un objeto de subred de Active Directory, la información acerca de subred IP de red y la máscara de subred se convierte automáticamente en el formato de notación de longitud de prefijo de red <IP address>/<prefix length>. Por ejemplo, la red IP versión 4 (IPv4) dirección 172.16.4.0 con una máscara de subred 255.255.252.0 aparece como 172.16.4.0/22. Además de las direcciones IPv4, Windows Server 2008 también admite IP versión 6 (IPv6) subred prefijos, por ejemplo, 3FFE:FFFF:0:C000:: / 64. Para obtener más información acerca de las subredes IP en cada ubicación, consulta la "Ubicaciones y subredes" (DSSTOPO_2.doc) hoja de cálculo [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) y [Apéndice A: ubicaciones y prefijos de subred ](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Asociar cada objeto de subred con un objeto de sitio haciendo referencia a la hoja de cálculo de (DSSTOPO_6.doc) "Asociar subredes con sitios" en "decidir que ubicaciones le se convierten en" sección sitios para determinar qué subred es asociarse con un sitio determinado. El objeto de subred de Active Directory asociado a cada ubicación en la hoja de cálculo de (DSSTOPO_6.doc) "Asociar subredes con sitios" del documento.  
  
Para obtener más información sobre cómo crear objetos de subred, consulta crear una subred ([https://go.microsoft.com/fwlink/?LinkId=107068](https://go.microsoft.com/fwlink/?LinkId=107068)).  
  


