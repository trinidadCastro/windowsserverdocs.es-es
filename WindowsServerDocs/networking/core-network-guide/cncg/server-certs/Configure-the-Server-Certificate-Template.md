---
title: Configurar la plantilla de certificado de servidor
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 8ff610e2-43ca-407f-a828-06d9366e02f0
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e747fedd74dfc55c2c4df457d7f107b618423b8e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-server-certificate-template"></a>Configurar la plantilla de certificado de servidor

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para configurar la plantilla de certificado que Active Directory&reg; servicios de certificados (AD CS) se usa como base para los certificados de servidor que están inscritos en los servidores de la red.  
  
Al configurar esta plantilla, puedes especificar los servidores por grupo de Active Directory que deben recibir automáticamente un certificado de servidor de AD CS.   
  
El procedimiento siguiente incluye instrucciones para configurar la plantilla para emitir certificados a todos los siguientes tipos de servidor:  
  
- Servidores que ejecutan el servicio de acceso remoto, incluidos los servidores de puerta de enlace de RAS, que son miembros de la **RAS y servidores IAS** grupo.  
- Servidores que ejecutan el servicio de servidor de directivas de redes (NPS) que son miembros de la **RAS y servidores IAS** grupo.  
  
Pertenencia a ambos **administradores** y el dominio raíz **administradores de dominio** grupo es lo mínimo necesario para completar este procedimiento.  
  
### <a name="to-configure-the-certificate-template"></a>Para configurar la plantilla de certificado  
  
1.  En CA1, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **entidad de certificación**. Abre la entidad de certificación de Microsoft Management Console (MMC).  
  
2.  En la consola de MMC, haga doble clic en el nombre de entidad emisora de certificados, haz clic en **plantillas de certificado**y, a continuación, haz clic en **administrar**.  
  
3.  Abre la consola de plantillas de certificado. Todas las plantillas de certificado se muestran en el panel de detalles.  
  
4.  En el panel de detalles, haz clic en el **RAS y servidor IAS** plantilla.  
  
5.  Haz clic en el **acción** menú y, a continuación, haz clic en **plantilla duplicada**. La plantilla **propiedades** abre el cuadro de diálogo.  
  
6.  Haz clic en el **seguridad** pestaña.   
  
7.  En la **seguridad** pestaña **nombres de grupo o usuario**, haz clic en **servidores RAS e IAS**.  
  
8.  En **permisos para servidores RAS e IAS**, en **permitir**, asegúrate de que **inscribir** está activada y, a continuación, selecciona el **inscripción automática** casilla de verificación. Haz clic en **Aceptar**y cierra MMC de plantillas de certificado.  
  
9.  En la consola de MMC de la entidad de certificación, haz clic en **plantillas de certificado**. En la **acción** menú, elija **nueva**y, a continuación, haz clic en **plantilla de certificado para emitir**. La **Habilitar plantillas de certificado** abre el cuadro de diálogo.  
  
10. En **Habilitar plantillas de certificado**, haga clic en el nombre de la plantilla de certificado que se ha configurado y, a continuación, haz clic en **Aceptar**. Por ejemplo, si no cambió el nombre de plantilla de certificado de forma predeterminada, haz clic en **RAS de copiar y servidor IAS**y, a continuación, haz clic en **Aceptar**.  
  


