---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Creación de un diseño de infraestructura de DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 080c36f8410be4d6b1933c74730e2b55ce8d0a0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856136"
---
# <a name="creating-a-dns-infrastructure-design"></a>Creación de un diseño de infraestructura de DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de crear los diseños de dominios y bosques de Active Directory, debe diseñar una infraestructura de sistema de nombres de dominio (DNS) para admitir la estructura lógica de Active Directory. DNS permite a los usuarios usar nombres descriptivos que sean fáciles de recordar para conectarse a equipos y otros recursos en redes IP. Servicios de dominio de Active Directory (AD DS) en Windows Server 2008 requiere DNS.  
  
El proceso de diseño de DNS para admitir AD DS varía según si su organización ya tiene un servicio de servidor DNS existente o va a implementar un nuevo servicio de servidor DNS:  
  
- Si ya tiene una infraestructura DNS existente, debe integrar el espacio de nombres de Active Directory en ese entorno. Para obtener más información, consulte [Integrating AD DS en una infraestructura DNS existente](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).  
- Si no tiene una infraestructura DNS en su lugar, debe diseñar e implementar una nueva infraestructura DNS para admitir AD DS. Para obtener más información, consulte [implementar el sistema de nombres de dominio (DNS)](https://go.microsoft.com/fwlink/?LinkId=93656).  
  
Si su organización tiene una infraestructura DNS existente, debe asegurarse de que comprende cómo interactúa la infraestructura de DNS con el espacio de nombres de Active Directory. Para que una hoja de cálculo que le ayudarán a documentar el diseño de infraestructura DNS existente, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip desde [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) y Abra "Inventario de DNS" (DSSLOGI_8.doc).  
  
> [!NOTE]  
> Además de IP versión 4 (IPv4) direcciones, direcciones de servidor de Windows también es compatible con IP versión 6 (IPv6). Para que una hoja de cálculo para ayudarle en el listado de las direcciones IPv6 al documentar el método de resolución de nombre recursiva de la estructura actual de DNS, consulte [Apéndice A: Inventario DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).
  
Antes de diseñar la infraestructura DNS para admitir AD DS, puede resultar útil obtener información acerca de la jerarquía DNS, el proceso de resolución de nombres DNS y cómo DNS es compatible con AD DS. Para obtener más información sobre el proceso de resolución de jerarquía y el nombre DNS, consulte la referencia técnica de DNS ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145)). Para obtener más información sobre la compatibilidad de DNS con AD DS, consulte la compatibilidad de DNS para la referencia técnica de Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
## <a name="in-this-section"></a>En esta sección  

- [Revisar los conceptos DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS y AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [Asignación de DNS para el rol de AD DS propietario](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [Integración de AD DS en una infraestructura DNS existente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
