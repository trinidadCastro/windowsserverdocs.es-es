---
description: Más información acerca de cómo agregar un registro de recursos de host (A) al DNS corporativo para un servidor de Federación
ms.assetid: 026747c7-4c34-41c7-b7ea-27f9a7f64a35
title: Agregar un registro de recursos de host (A) al DNS corporativo para un servidor de federación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 00e2366de66eced2ad0cbe3a731f5f6afceb18f6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047923"
---
# <a name="add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Agregar un registro de recursos de host (A) al DNS corporativo para un servidor de federación



Para que los clientes de la red corporativa tengan acceso correctamente a un servidor de Federación mediante la autenticación integrada de Windows, \( \) primero se debe crear un registro de recurso de host en el DNS del sistema de nombres de dominio corporativo \( \) que resuelva el nombre de host del servidor de Federación de la cuenta \( , por ejemplo, FS.fabrikam.com \) en la dirección IP del servidor de Federación o del clúster de servidores de Federación. Puede usar el siguiente procedimiento para agregar un registro de \( recurso de host \) a a DNS corporativo para un servidor de Federación.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

### <a name="to-add-a-host-a-resource-record-to-corporate-dns-for-a-federation-server"></a>Para agregar un registro de recurso de host \( \) a a DNS corporativo para un servidor de Federación

1.  En un servidor DNS de la red corporativa, abra el complemento DNS \- .

2.  En el árbol de consola, \- haga clic con el botón secundario en la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo host \( a o AAAA \)**.

3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación o del clúster de servidores de Federación; por ejemplo, para el nombre de dominio completo \( FQDN \) FS.fabrikam.com, escriba **FS**.

4.  En **dirección IP**, escriba la dirección IP del servidor de Federación o del clúster de servidores de Federación, por ejemplo, 192.168.1.4.

5.  Haz clic en **Agregar host**.

## <a name="additional-references"></a>Referencias adicionales
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)

[Requisitos de resolución de nombres para servidores de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))

