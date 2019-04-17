---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zonas de DNS Active Directory integrado
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 11940ddda320ea36bf8439afed91fcf6803f2fee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-integrated-dns-zones"></a>Zonas de DNS Active Directory integrado

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servidores de dominio nombre DNS (sistema) que se ejecutan en los controladores de dominio pueden almacenar sus zonas en los servicios de dominio de Active Directory (AD DS). De este modo, no es necesario configurar una topología de replicación de DNS independiente que utiliza ordinarias transferencias de zonas DNS, porque todos los datos de la zona se replica automáticamente mediante la replicación de Active Directory. Esto simplifica el proceso de implementación de DNS y proporciona las siguientes ventajas:  
  
-   Se crean varios patrones para la replicación de DNS. Por lo tanto, cualquier controlador de dominio del dominio que se ejecuta el servicio de servidor DNS puede escribir las actualizaciones a las zonas DNS integrado en Active Directory, el nombre de dominio para los que están autorizados. No se necesita una topología de transferencia de zona DNS independiente.  
  
-   Se admiten las actualizaciones dinámicas seguras. Las actualizaciones dinámicas seguras permiten a los administradores controlar qué equipos actualizan qué nombres e impedir que los equipos no autorizados sobrescribir nombres existentes en DNS.  
  
DNS de Active Directory integrado en Windows Server 2008 almacena los datos de la zona en particiones de directorio de la aplicación. (No hay ningún cambio de comportamiento de la integración de DNS basado en Windows Server 2003 con Active Directory). Las siguientes particiones de directorio de aplicación específicas de DNS se crean durante la instalación de AD DS:  
  
-   Una partición de directorio de aplicación de todo el bosque denominada ForestDnsZones  
  
-   Aplicación de todo el dominio de particiones de directorio para cada dominio en el bosque, denominado DomainDnsZones  
  
Para obtener más información acerca de cómo AD DS almacena la información de DNS en particiones de aplicaciones, consulta la [referencia técnica de DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Te recomendamos que instales DNS al ejecutar el Asistente para la instalación a servicios de Active Directory dominio de (Dcpromo.exe). Si haces esto, el asistente crea automáticamente la delegación de zona DNS. Para obtener más información, consulta [implementar Windows Server 2008 dominio raíz del bosque](https://technet.microsoft.com/library/cc731174.aspx).  
  


