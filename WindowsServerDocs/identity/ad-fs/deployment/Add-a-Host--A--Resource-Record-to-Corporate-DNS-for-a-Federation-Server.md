---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: "Agregar un registro de recursos de Host (A) al DNS corporativa para un servidor de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 425cfe794095f1515eb3fae2f1a5e5db90ba3d00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Agregar un registro de recursos de Host (A) al DNS corporativa para un servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Para los clientes de la empresa de red para acceder correctamente a un servidor de federación mediante la autenticación integrada de Windows, primero deben crearse un registro de host \(A\) recursos en el \(DNS\) sistema de nombres de dominio corporativo que resuelve el nombre de host del servidor de federación de cuenta \ (por ejemplo, fs.fabrikam.com\) a la dirección IP del servidor de federación o clúster de servidor de federación. Puedes usar el siguiente procedimiento para agregar un registro de host \(A\) recursos corporativos DNS para un servidor de federación.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para agregar un registro de host \(A\) recursos corporativos DNS para un servidor de federación  
  
1.  En un servidor DNS para la red corporativa, abre el DNS en snap\.  
  
2.  En la consola de árbol, right\ y haga clic en la zona de búsqueda directa correspondiente y, a continuación, haz clic en **nuevo Host \(A or AAAA\)**.  
  
3.  En **nombre**, escribe el nombre de equipo del servidor de federación o del clúster de servidor de federación; Por ejemplo, para el dominio completo nombre \(FQDN\) fs.fabrikam.com, tipo **fs**.  
  
4.  En **dirección IP**, escribe la dirección IP del servidor de federación o clúster de servidor de federación, por ejemplo, 192.168.1.4.  
  
5.  Haz clic en **agregar Host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de resolución de nombres para los servidores de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

