---
title: Configurar la inscripción automática del certificado del servidor
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ddf8a905fdb68bbc474b10f526b32f3d8b83af46
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-auto-enrollment"></a>Configurar la inscripción automática de certificados

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

> [!NOTE]
> Antes de realizar este procedimiento, debe configurar una plantilla de certificado de servidor mediante el complemento Microsoft Management Console de plantillas de certificado en una entidad de certificación que se ejecuta AD CS.
Pertenencia a ambos **administradores** y el dominio raíz **administradores de dominio** grupo es lo mínimo necesario para completar este procedimiento.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurar la inscripción automática del certificado del servidor

1. En el equipo donde está instalado AD DS, abre Windows PowerShell&reg;, tipo **mmc**, y, a continuación, presione ENTRAR. Abre la consola de administración de Microsoft.
2. En la **archivo** menú, haz clic en **agregar o quitar complemento**. La **agregar o quitar complementos** abre el cuadro de diálogo.
3. En **complementos disponibles**, desplácese hacia abajo y haz doble clic en **el Editor de administración de directivas de grupo**. La **seleccionar un objeto de directiva de grupo** abre el cuadro de diálogo.

     > [!IMPORTANT]
     > Asegúrate de que seleccionas **el Editor de administración de directivas de grupo** y no **Group Policy Management**. Si seleccionas **Group Policy Management**, se producirá un error en la configuración con estas instrucciones y un certificado de servidor no se inscribe automáticamente a los servidores NPS.

4. En **objeto de directiva de grupo**, haz clic en **examinar**. La **buscar un objeto de directiva de grupo** abre el cuadro de diálogo.
5. En **dominios, unidades organizativas y objetos de directiva de grupo vinculados,** haga clic en **directiva de dominio predeterminada**y, a continuación, haz clic en **Aceptar**.
6. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar**.
7. Haz doble clic en **directiva de dominio predeterminada**. En la consola, expande la ruta de acceso siguiente: **configuración del equipo**, **directivas**, **configuración de Windows**, **la configuración de seguridad**y luego **directivas de clave pública**.
8. Haz clic en **directivas de clave pública**. En el panel de detalles, haz doble clic en **cliente de servicios de certificados: inscripción automática**. La **propiedades** abre el cuadro de diálogo. Configurar los siguientes elementos y, a continuación, haz clic en **Aceptar**:

     1. En **modelo de configuración**, selecciona **habilitado**.
     2. Selecciona el **renovar certificados caducados, actualizar certificados pendientes y quitar revocado certificados** casilla de verificación.
     3. Selecciona el **actualizar certificados que usan plantillas de certificado** casilla de verificación.

9. Haz clic en **Aceptar**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurar la inscripción automática del certificado del usuario

1. En el equipo donde está instalado AD DS, abre Windows PowerShell&reg;, tipo **mmc**, y, a continuación, presione ENTRAR. Abre la consola de administración de Microsoft.
2. En la **archivo** menú, haz clic en **agregar o quitar complemento**. La **agregar o quitar complementos** abre el cuadro de diálogo.
3. En **complementos disponibles**, desplácese hacia abajo y haz doble clic en **el Editor de administración de directivas de grupo**. La **seleccionar un objeto de directiva de grupo** abre el cuadro de diálogo.

     > [!IMPORTANT]
     > Asegúrate de que seleccionas **el Editor de administración de directivas de grupo** y no **Group Policy Management**. Si seleccionas **Group Policy Management**, se producirá un error en la configuración con estas instrucciones y un certificado de servidor no se inscribe automáticamente a los servidores NPS.

4. En **objeto de directiva de grupo**, haz clic en **examinar**. La **buscar un objeto de directiva de grupo** abre el cuadro de diálogo.
5. En **dominios, unidades organizativas y objetos de directiva de grupo vinculados,** haga clic en **directiva de dominio predeterminada**y, a continuación, haz clic en **Aceptar**.
6. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar**.
7. Haz doble clic en **directiva de dominio predeterminada**. En la consola, expande la ruta de acceso siguiente: **configuración de usuario**, **directivas**, **configuración de Windows**, **la configuración de seguridad**y luego **directivas de clave pública**.
8. Haz clic en **directivas de clave pública**. En el panel de detalles, haz doble clic en **cliente de servicios de certificados: inscripción automática**. La **propiedades** abre el cuadro de diálogo. Configurar los siguientes elementos y, a continuación, haz clic en **Aceptar**:

     1. En **modelo de configuración**, selecciona **habilitado**.
     2. Selecciona el **renovar certificados caducados, actualizar certificados pendientes y quitar revocado certificados** casilla de verificación.
     3. Selecciona el **actualizar certificados que usan plantillas de certificado** casilla de verificación.

9. Haz clic en **Aceptar**.

## <a name="next-steps"></a>Pasos siguientes

[Directiva de grupo de actualización](refresh-group-policy.md)