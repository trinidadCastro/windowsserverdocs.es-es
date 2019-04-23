---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zonas DNS integradas de Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5e0830944ec5d91dace52db337e89211ee362b98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873256"
---
# <a name="active-directory-integrated-dns-zones"></a>Zonas DNS integradas de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de sistema de nombres (DNS) del dominio que se ejecutan en controladores de dominio pueden almacenar sus zonas en servicios de dominio de Active Directory (AD DS). De este modo, no es necesario configurar una topología de replicación de DNS independiente que usa las transferencias de zona DNS normales porque todos los datos de la zona se replica automáticamente mediante la replicación de Active Directory. Esto simplifica el proceso de implementación de DNS y proporciona las siguientes ventajas:  
  
-   Se crean varios patrones para la replicación de DNS. Por lo tanto, cualquier controlador de dominio en el dominio que ejecuta el servicio servidor DNS puede escribir las actualizaciones a las zonas DNS integradas en Active Directory para el nombre de dominio para el que están autorizados. No es necesaria una topología independiente de transferencia de zona DNS.  
  
-   Se admiten las actualizaciones dinámicas seguras. Las actualizaciones dinámicas seguras permiten al administrador controlar qué equipos actualizan qué nombres y evitar que equipos no autorizados sobrescriba los nombres existentes en DNS.  
  
DNS integrado en Active Directory en Windows Server 2008 almacena datos de zona en particiones de directorio de aplicación. (No hay ningún cambio de comportamiento de la integración de DNS basado en Windows Server 2003 con Active Directory). Las siguientes particiones de directorio de aplicaciones específicas de DNS se crean durante la instalación de AD DS:  
  
-   Una partición de directorio de aplicación de todo el bosque, denominada ForestDnsZones  
  
-   Particiones de directorio de aplicación de todo el dominio para cada dominio del bosque, denominado DomainDnsZones  
  
Para obtener más información sobre cómo AD DS almacena información de DNS en las particiones de aplicación, consulte el [referencia técnica de DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Se recomienda instalar DNS al ejecutar el Asistente de instalación de servicios de dominio de Active Directory (Dcpromo.exe). Si lo hace, el asistente crea automáticamente la delegación de zona DNS. Para obtener más información, consulte [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


