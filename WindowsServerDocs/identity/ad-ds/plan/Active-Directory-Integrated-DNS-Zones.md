---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zonas DNS integradas de Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5e9047fced5c89c1f2c9d5edaf1ff02536c2a709
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822898"
---
# <a name="active-directory-integrated-dns-zones"></a>Zonas DNS integradas de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servidores del sistema de nombres de dominio (DNS) que se ejecutan en controladores de dominio pueden almacenar sus zonas en Active Directory Domain Services (AD DS). De esta manera, no es necesario configurar una topología de replicación de DNS independiente que use transferencias de zona DNS normales, ya que todos los datos de la zona se replican automáticamente por medio de la replicación de Active Directory. Esto simplifica el proceso de implementación de DNS y proporciona las siguientes ventajas:  
  
-   Se crean varios patrones para la replicación de DNS. Por lo tanto, cualquier controlador de dominio del dominio que ejecute el servicio servidor DNS puede escribir actualizaciones en las zonas DNS integradas en Active Directory para el nombre de dominio para el que son autoritativos. No es necesaria una topología de transferencia de zona DNS independiente.  
  
-   Se admiten las actualizaciones dinámicas seguras. Las actualizaciones dinámicas seguras permiten a un administrador controlar qué equipos actualizan los nombres y evitar que los equipos no autorizados sobrescriban los nombres existentes en DNS.  
  
Active Directory DNS integrado en Windows Server 2008 almacena datos de zona en particiones de directorio de aplicaciones. (No hay cambios de comportamiento de la integración de DNS basada en Windows Server 2003 con Active Directory). Las siguientes particiones de directorio de aplicaciones específicas de DNS se crean durante la instalación de AD DS:  
  
-   Una partición de directorio de aplicaciones en todo el bosque, denominada ForestDnsZones  
  
-   Particiones de directorio de aplicaciones para todo el dominio para cada dominio del bosque, denominado DomainDnsZones  
  
Para obtener más información sobre cómo AD DS almacena la información de DNS en las particiones de aplicación, consulte la [referencia técnica de DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Se recomienda instalar DNS al ejecutar el Asistente para instalación de Active Directory Domain Services (Dcpromo. exe). Si lo hace, el asistente creará automáticamente la delegación de la zona DNS. Para obtener más información, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


