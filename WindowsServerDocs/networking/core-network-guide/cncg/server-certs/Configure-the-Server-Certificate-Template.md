---
title: Configurar la plantilla de certificado de servidor
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fc4ba05466c2d87e6d6e9f7c0d5cee1bb405c79a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318391"
---
# <a name="configure-the-server-certificate-template"></a>Configurar la plantilla de certificado de servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para configurar la plantilla de certificado que Active Directory&reg; servicios de Certificate Server (AD CS) utiliza como base para los certificados de servidor que están inscritos en los servidores de la red.  
  
Al configurar esta plantilla, puede especificar los servidores por Active Directory grupo que debe recibir automáticamente un certificado de servidor de AD CS.   
  
En el procedimiento siguiente se incluyen instrucciones para configurar la plantilla con el fin de emitir certificados para todos los tipos de servidor siguientes:  
  
- Servidores que ejecutan el servicio de acceso remoto, incluidos los servidores de puerta de enlace RAS, que son miembros del grupo de **servidores RAS e IAS** .  
- Servidores que ejecutan el servicio del servidor de directivas de redes (NPS) y que son miembros del grupo de **servidores RAS e IAS** .  
  
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Administradores de empresas** y al grupo **Admins. del dominio** del dominio raíz.  
  
### <a name="to-configure-the-certificate-template"></a>Para configurar la plantilla de certificado  
  
1.  En CA1, en Administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **entidad de certificación**. Se abre Microsoft Management Console (MMC) de la entidad de certificación.  
  
2.  En MMC, haga doble clic en el nombre de la entidad de certificación, haga clic con el botón secundario en **plantillas de certificado**y, a continuación, haga clic en **administrar**.  
  
3.  Se abre la consola de plantillas de certificado. Se mostrarán todas las plantillas de certificado en el panel de detalles.  
  
4.  En el panel de detalles, haga clic en la plantilla **servidor RAS e IAS** .  
  
5.  Haga clic en el menú **acción** y, a continuación, haga clic en **plantilla duplicada**. Se abrirá el cuadro de diálogo **propiedades** de la plantilla.  
  
6.  Haga clic en la pestaña **Security**.   
  
7.  En la pestaña **seguridad** , en **nombres de grupos o usuarios**, haga clic en **servidores RAS e IAS**.  
  
8.  En **permisos para servidores RAS e IAS**, en **permitir**, asegúrese de que la opción **inscribir** está seleccionada y, a continuación, active la casilla **inscripción automática** . Haga clic en **Aceptar**y cierre la MMC plantillas de certificado.  
  
9.  En MMC de la entidad de certificación, haga clic en **plantillas de certificado**. En el menú **acción** , seleccione **nuevo**y, a continuación, haga clic en **plantilla de certificado que se va a emitir**. Se abre el cuadro de diálogo **Habilitar plantillas de certificados**.  
  
10. En **Habilitar plantillas de certificado**, haga clic en el nombre de la plantilla de certificado que acaba de configurar y, a continuación, haga clic en **Aceptar**. Por ejemplo, si no cambió el nombre de la plantilla de certificado predeterminada, haga clic en **copia de RAS e IAS Server**y, a continuación, haga clic en **Aceptar**.  
  


