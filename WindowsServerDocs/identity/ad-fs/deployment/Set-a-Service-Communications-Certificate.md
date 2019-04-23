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
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888476"
---
# <a name="set-a-service-communications-certificate"></a>Establecer un certificado de comunicaciones de servicio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de federación de Active Directory Federation Services \(AD FS\) usar el certificado de comunicaciones de servicio para proteger el tráfico de servicios Web para capa de Sockets seguros \(SSL\) comunicación con Web los clientes o con servidores proxy de federación. Este es el mismo certificado que usa un servidor de federación como el certificado SSL en Internet Information Services \(IIS\).  
  
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
[Lista de comprobación: Cómo configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Requisitos de certificados para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
  

