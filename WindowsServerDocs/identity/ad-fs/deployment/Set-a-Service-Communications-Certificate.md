---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Establecer un certificado de comunicaciones de servicio
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 140e8e4204148dd8862385054554d7b8336856ec
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192012"
---
# <a name="set-a-service-communications-certificate"></a>Establecer un certificado de comunicaciones de servicio


Servidores de federación de Active Directory Federation Services \(AD FS\) usar el certificado de comunicaciones de servicio para proteger el tráfico de servicios Web para capa de Sockets seguros \(SSL\) comunicación con Web los clientes o con servidores proxy de federación.

> [!NOTE]  
> El certificado de comunicaciones de servicio no es igual que un certificado SSL. Para cambiar el certificado SSL de AD FS, necesitará usar Powershell. Siga las instrucciones de este [artículo](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


Puede usar el procedimiento siguiente para cambiar el certificado de comunicaciones de servicio con el complemento Administración de AD FS\-en.  

> [!NOTE]  
> El complemento de administración de AD FS\-en hace referencia a certificados de autenticación de servidor para los servidores de federación como certificados de comunicación de servicio.  

El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Para establecer un certificado de comunicaciones de servicio  

1.  En el **iniciar** , escriba**administración de AD FS**, y, a continuación, presione ENTRAR.  

2.  En el árbol de consola, haga doble\-haga clic en **servicio**y, a continuación, haga clic en **certificados**.  

3.  En el **acciones** panel, haga clic en el **establecer certificado de comunicaciones de servicio** vínculo.  

4.  En el **seleccionar un certificado de comunicaciones de servicio** diálogo cuadro, vaya al archivo de certificado que desea establecer como el certificado de comunicaciones de servicio, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.  

## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
