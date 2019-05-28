---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Agregar un registro de recursos de host (A) al DNS corporativo para un servidor de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5767fa45f8b25680aa1b1d97ddab630923d10fae
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192491"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Agregar un registro de recursos de host (A) al DNS corporativo para un servidor de federación



Para los clientes de la empresa de red tenga acceso correctamente a un servidor de federación mediante la autenticación integrada de Windows, un host \(A\) primero se debe crear el registro de recursos en el sistema de nombres de dominio corporativo \(DNS\) que resuelve el nombre de host del servidor de federación de cuenta \(por ejemplo, fs.fabrikam.com\) a la dirección IP del servidor de federación o el clúster de servidores de federación. Puede usar el siguiente procedimiento para agregar un host \(A\) registro de recurso al DNS corporativo para un servidor de federación.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para agregar un host \(A\) registro de recurso al DNS corporativo para un servidor de federación  
  
1.  En un servidor DNS para la red corporativa, abra el complemento DNS\-en.  
  
2.  En el árbol de consola, a la derecha\-haga clic en la zona de búsqueda directa correspondiente y, a continuación, haga clic en **nuevo Host \(A o AAAA\)** .  
  
3.  En **nombre**, escriba el nombre del equipo del servidor de federación o el clúster de servidores de federación; por ejemplo, para el nombre de dominio completo \(FQDN\) fs.fabrikam.com, escriba **fs**.  
  
4.  En **dirección IP**, escriba la dirección IP para el servidor de federación o el clúster de servidores de federación, por ejemplo, 192.168.1.4.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de resolución de nombres para servidores de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

