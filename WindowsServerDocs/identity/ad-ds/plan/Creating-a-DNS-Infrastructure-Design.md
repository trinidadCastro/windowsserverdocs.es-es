---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: "Crear un diseño de la infraestructura DNS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6e5093b05fd81a693cec87ddb00d39e70483df23
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-dns-infrastructure-design"></a>Crear un diseño de la infraestructura DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de crear los diseños de bosque y dominio de Active Directory, debes diseñar una infraestructura de sistema de nombres de dominio (DNS) para admitir la estructura lógica de Active Directory. DNS permite a los usuarios usar nombres descriptivos que son fáciles de recordar para conectarse a equipos y otros recursos en redes IP. Servicios de dominio de Active Directory (AD DS) en Windows Server 2008 requiere DNS.  
  
El proceso de diseño de DNS para admitir AD DS varía según si la organización ya tiene un servicio de servidor DNS existente o vas a implementar un nuevo servicio de servidor DNS:  
  
-   Si ya tienes una infraestructura de DNS, debes integrar el espacio de nombres de Active Directory en ese entorno. Para obtener más información, consulta [integración AD DS en una infraestructura DNS](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
  
-   Si no tienes una infraestructura DNS en su lugar, debes diseñar e implementar una nueva infraestructura DNS para admitir AD DS. Para obtener más información, consulta implementar el sistema de nombres de dominio (DNS) ([https://go.microsoft.com/fwlink/?LinkId=93656](https://go.microsoft.com/fwlink/?LinkId=93656)).  
  
Si tu organización tiene una infraestructura de DNS, debes asegurarte de que conoces cómo interactúa la infraestructura DNS con el espacio de nombres de Active Directory. Para que una hoja de cálculo que le ayudarán a documentar el diseño actual de la infraestructura DNS, descarga Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) y abre "DNS inventario" (DSSLOGI_8.doc).  
  
> [!NOTE]  
> Además de IP versión 4 (IPv4) direcciones, direcciones de Windows Server 2008 también admite IP versión 6 (IPv6). Para que una hoja de cálculo que le ayudarán a la descripción de las direcciones IPv6 mientras documentar el método de resolución de nombre recursivo de la estructura DNS actual, consulta [Apéndice A: DNS inventario](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).  
  
Antes de diseñar la infraestructura DNS para admitir AD DS, puede resultar útil obtener información acerca de la jerarquía DNS, el proceso de resolución de nombre DNS y cómo DNS es compatible con AD DS. Para obtener más información sobre el proceso de resolución de jerarquía y el nombre DNS, consulta la referencia técnica de DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Para obtener más información acerca de cómo DNS es compatible con AD DS, consulta la compatibilidad de DNS para la referencia técnica de Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revisar los conceptos de DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
  
-   [DNS y AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
  
-   [Asignar el DNS para el rol de AD DS propietario](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
  
-   [Integración de AD DS en una infraestructura DNS](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
  


