---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Establecer un certificado de comunicaciones de servicio
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d0464853c73f88ed76545921ffc8a4bf8551c800
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408325"
---
# <a name="set-a-service-communications-certificate"></a>Establecer un certificado de comunicaciones de servicio


Servidores de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 Use el certificado de comunicaciones de servicio para proteger el tráfico de servicios web para Capa de sockets seguros la comunicación \(SSL @ no__t-3 con clientes Web o con el servidor de Federación. servidores proxy.

> [!NOTE]  
> El certificado de comunicaciones de servicio no es el mismo que un certificado SSL. Para cambiar el certificado SSL AD FS, debe usar PowerShell. Siga las instrucciones de este [artículo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap).


Puede usar el siguiente procedimiento para cambiar el certificado de comunicaciones de servicio con el AD FS de administración de @ no__t-0in.  

> [!NOTE]  
> El complemento de administración de AD FS @ no__t-0in hace referencia a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.  

El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en \( [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   

### <a name="to-set-a-service-communications-certificate"></a>Para establecer un certificado de comunicaciones de servicio  

1.  En la pantalla **Inicio** , escriba**AD FS Management**y, a continuación, presione Entrar.  

2.  En el árbol de consola, duplique el **servicio**@ no__t-0click y, a continuación, haga clic en **certificados**.  

3.  En el panel **acciones** , haga clic en el vínculo **establecer certificado de comunicaciones de servicio** .  

4.  En el cuadro de diálogo **seleccionar un certificado de comunicaciones de servicio** , navegue hasta el archivo de certificado que desea establecer como certificado de comunicaciones de servicio, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.  

## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)  

[Requisitos de certificado para servidores de federación](https://technet.microsoft.com/library/dd807040.aspx)  
