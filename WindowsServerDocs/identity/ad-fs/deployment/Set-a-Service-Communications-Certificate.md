---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Establecer un certificado de comunicaciones de servicio
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6549b120768bf01c43f38fc5252529e971043eaa
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956057"
---
# <a name="set-a-service-communications-certificate"></a>Establecer un certificado de comunicaciones de servicio


Los servidores de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) usar el certificado de comunicaciones de servicio para proteger el tráfico de servicios web para capa de sockets seguros la \( \) comunicación SSL con clientes Web o con servidores proxy de Federación.

> [!NOTE]  
> El certificado de comunicaciones de servicio no es el mismo que un certificado SSL. Para cambiar el certificado SSL AD FS, debe usar PowerShell. Siga las instrucciones de este [artículo](../operations/manage-ssl-certificates-ad-fs-wap.md).


Puede usar el siguiente procedimiento para cambiar el certificado de comunicaciones de servicio con el complemento \- de administración de AD FS en.  

> [!NOTE]  
> El complemento de administración \- de AD FS en se refiere a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.  

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .   

### <a name="to-set-a-service-communications-certificate"></a>Para establecer un certificado de comunicaciones de servicio  

1.  En la pantalla **Inicio** , escriba**AD FS Management**y, a continuación, presione Entrar.  

2.  En el árbol de consola, haga doble \- clic en **servicio**y, a continuación, haga clic en **certificados**.  

3.  En el panel **acciones** , haga clic en el vínculo **establecer certificado de comunicaciones de servicio** .  

4.  En el cuadro de diálogo **seleccionar un certificado de comunicaciones de servicio** , navegue hasta el archivo de certificado que desea establecer como certificado de comunicaciones de servicio, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.  

## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisitos de certificado para servidores de federación](../design/certificate-requirements-for-federation-servers.md)  
