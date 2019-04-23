---
title: Configurar la plantilla de certificado de servidor
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 238579c945821d19e45dad7e623450d598830596
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886126"
---
# <a name="configure-the-server-certificate-template"></a>Configurar la plantilla de certificado de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar la plantilla de certificado que Active Directory&reg; Certificate Services (AD CS) usan como base para los certificados de servidor que están inscritos en los servidores de la red.  
  
Al configurar esta plantilla, puede especificar los servidores por grupo de Active Directory que debe recibir automáticamente un certificado de servidor de AD CS.   
  
El siguiente procedimiento incluye instrucciones para configurar la plantilla para emitir certificados para todos los tipos de servidor siguientes:  
  
- Servidores que ejecutan el servicio de acceso remoto, incluidos los servidores de puerta de enlace RAS, que son miembros de la **servidores RAS e IAS** grupo.  
- Los servidores que ejecutan el servicio servidor de directivas de redes (NPS) que son miembros de la **servidores RAS e IAS** grupo.  
  
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Administradores de empresas** y al grupo **Admins. del dominio** del dominio raíz.  
  
### <a name="to-configure-the-certificate-template"></a>Para configurar la plantilla de certificado  
  
1.  En CA1, en el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **entidad de certificación**. Se abre la autoridad de certificación de Microsoft Management Console (MMC).  
  
2.  En MMC, haga doble clic en el nombre de entidad de certificación, haga clic en **plantillas de certificado**y, a continuación, haga clic en **administrar**.  
  
3.  Se abre la consola de plantillas de certificado. Todas las plantillas de certificado se muestran en el panel de detalles.  
  
4.  En el panel de detalles, haga clic en el **servidor RAS e IAS** plantilla.  
  
5.  Haga clic en el **acción** menú y, a continuación, haga clic en **Duplicar plantilla**. La plantilla **propiedades** abre el cuadro de diálogo.  
  
6.  Haga clic en la pestaña **Seguridad**.   
  
7.  En el **seguridad** ficha **los nombres de usuario o grupo**, haga clic en **servidores RAS e IAS**.  
  
8.  En **permisos de servidores RAS e IAS**, en **permitir**, asegúrese de que **inscribir** está seleccionado y, a continuación, seleccione el **inscripción automática** comprobar cuadro. Haga clic en **Aceptar**y cierre la MMC de plantillas de certificado.  
  
9.  En la MMC de entidad de certificación, haga clic en **plantillas de certificado**. En el **acción** menú, elija **New**y, a continuación, haga clic en **plantilla de certificado para emitir**. Se abre el cuadro de diálogo **Habilitar plantillas de certificados**.  
  
10. En **Habilitar plantillas de certificados**, haga clic en el nombre de la plantilla de certificado que acaba de configurar y, a continuación, haga clic en **Aceptar**. Por ejemplo, si no ha cambiado el nombre de plantilla de certificado predeterminado, haga clic en **copia de RAS e IAS Server**y, a continuación, haga clic en **Aceptar**.  
  


