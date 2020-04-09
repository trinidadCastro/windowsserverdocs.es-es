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
ms.openlocfilehash: 6e0f9e6cca4fe915d3faed77fd5b5db543596d70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855308"
---
# <a name="set-a-service-communications-certificate"></a>Establecer un certificado de comunicaciones de servicio


Los servidores de Federación de Servicios de federación de Active Directory (AD FS) \(AD FS\) usar el certificado de comunicaciones de servicio para proteger el tráfico de servicios web para Capa de sockets seguros \(SSL\) la comunicación con clientes Web o con servidores proxy de Federación.

> [!NOTE]  
> El certificado de comunicaciones de servicio no es el mismo que un certificado SSL. Para cambiar el certificado SSL AD FS, debe usar PowerShell. Siga las instrucciones de este [artículo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


Puede usar el siguiente procedimiento para cambiar el certificado de comunicaciones de servicio con el\-de AD FS de administración en.  

> [!NOTE]  
> El\-de administración de AD FS en se refiere a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.  

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Para establecer un certificado de comunicaciones de servicio  

1.  En la pantalla **Inicio** , escriba**AD FS Management**y, a continuación, presione Entrar.  

2.  En el árbol de consola, haga doble\-haga clic en **servicio**y, a continuación, haga clic en **certificados**.  

3.  En el panel **acciones** , haga clic en el vínculo **establecer certificado de comunicaciones de servicio** .  

4.  En el cuadro de diálogo **seleccionar un certificado de comunicaciones de servicio** , navegue hasta el archivo de certificado que desea establecer como certificado de comunicaciones de servicio, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.  

## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor de Federación](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
