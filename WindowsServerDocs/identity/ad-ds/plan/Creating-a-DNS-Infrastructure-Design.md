---
description: Más información acerca de cómo crear un diseño de infraestructura de DNS
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: Creación de un diseño de infraestructura de DNS
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: b9dc73b44fcfc104f8e96a7e693c3867d8072cf4
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050003"
---
# <a name="creating-a-dns-infrastructure-design"></a>Creación de un diseño de infraestructura de DNS

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de crear el bosque de Active Directory y los diseños de dominio, debe diseñar una infraestructura del sistema de nombres de dominio (DNS) para admitir su Active Directory estructura lógica. DNS permite que los usuarios usen nombres descriptivos fáciles de recordar para conectarse a equipos y otros recursos en redes IP. Active Directory Domain Services (AD DS) en Windows Server 2008 requiere DNS.

El proceso de diseño de DNS para admitir AD DS varía en función de si su organización ya tiene un servicio de servidor DNS existente o si está implementando un nuevo servicio de servidor DNS:

- Si ya tiene una infraestructura DNS existente, debe integrar el espacio de nombres Active Directory en ese entorno. Para obtener más información, vea [integrar AD DS en una infraestructura DNS existente](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md).
- Si no tiene una infraestructura DNS, debe diseñar e implementar una nueva infraestructura DNS para admitir AD DS. Para obtener más información, consulte [implementación del sistema de nombres de dominio (DNS)](/previous-versions/windows/it-pro/windows-server-2003/cc780661(v=ws.10)).

Si su organización tiene una infraestructura DNS existente, debe asegurarse de que comprende cómo la infraestructura DNS interactuará con el espacio de nombres Active Directory. Para obtener una hoja de cálculo que le ayude a documentar el diseño de la infraestructura de DNS existente, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip del [Kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) y abra el "inventario de DNS" (DSSLOGI_8.doc).

> [!NOTE]
> Además de las direcciones IP versión 4 (IPv4), Windows Server también admite direcciones IP versión 6 (IPv6). Para ver una hoja de cálculo que le ayude a enumerar las direcciones IPv6 al documentar el método de resolución de nombres recursivo de la estructura DNS actual, consulte [apéndice a: inventario de DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md).

Antes de diseñar la infraestructura DNS para admitir AD DS, puede ser útil leer información acerca de la jerarquía de DNS, el proceso de resolución de nombres DNS y el modo en que DNS admite AD DS. Para obtener más información acerca de la jerarquía de DNS y el proceso de resolución de nombres, consulte la [referencia técnica de DNS](/previous-versions/windows/it-pro/windows-server-2003/cc779926(v=ws.10)). Para obtener más información sobre cómo DNS admite AD DS, consulte la [referencia técnica de compatibilidad de DNS con Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10)).

## <a name="in-this-section"></a>En esta sección

- [Revisar los conceptos de DNS](../../ad-ds/plan/Reviewing-DNS-Concepts.md)
- [DNS y AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)
- [Asignar el DNS al rol de propietario de AD DS](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)
- [Integrar AD DS en una infraestructura de DNS existente](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)
