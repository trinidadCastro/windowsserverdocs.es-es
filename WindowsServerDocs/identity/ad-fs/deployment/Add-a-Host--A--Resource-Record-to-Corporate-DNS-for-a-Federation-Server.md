---
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Agregar un registro de recursos de host (A) al DNS corporativo para un servidor de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 132e71cec134d17dd73be998683c09f752fdc414
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360332"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Agregar un registro de recursos de host (A) al DNS corporativo para un servidor de federación



Para que los clientes de la red corporativa tengan acceso correctamente a un servidor de Federación mediante la autenticación integrada de Windows, primero se debe crear un registro de recursos de host \(A @ no__t-1 en el sistema de nombres de dominio corporativo \(DNS @ no__t-3, que resuelve el nombre de host del servidor de Federación de la cuenta \(for ejemplo, FS. fabrikam. com @ no__t-5 a la dirección IP del servidor de Federación o del clúster de servidores de Federación. Puede usar el siguiente procedimiento para agregar un registro de recursos de host \(A @ no__t-1 al DNS corporativo de un servidor de Federación.  
  
La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para agregar un registro de recursos de host \(A @ no__t-1 al DNS corporativo de un servidor de Federación  
  
1.  En un servidor DNS de la red corporativa, abra el paso de DNS "no__t-0in".  
  
2.  En el árbol de consola, haga clic en @ no__t-0click la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo Host \(A o AAAA @ no__t-3**.  
  
3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación o del clúster de servidores de Federación; por ejemplo, para el nombre de dominio completo @no__t 1FQDN @ no__t-2 fs.fabrikam.com, escriba **FS**.  
  
4.  En **dirección IP**, escriba la dirección IP del servidor de Federación o del clúster de servidores de Federación, por ejemplo, 192.168.1.4.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de resolución de nombres para servidores de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

