---
title: Configurar la inscripción automática del certificado del servidor
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea7b7efe01525f4ecfb35200463c3f221d92d62d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888566"
---
# <a name="configure-certificate-auto-enrollment"></a>Configurar la inscripción automática de certificados

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!NOTE]
> Antes de realizar este procedimiento, debe configurar una plantilla de certificado de servidor mediante el complemento Microsoft Management Console de plantillas de certificado en una CA que ejecuta AD CS.
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Administradores de empresas** y al grupo **Admins. del dominio** del dominio raíz.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurar la inscripción automática del certificado del servidor

1. En el equipo donde está instalado AD DS, abra Windows PowerShell&reg;, tipo **mmc**, y, a continuación, presione ENTRAR. Se abre Microsoft Management Console.
2. En el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.
3. En **complementos disponibles**, desplácese hacia abajo y haga doble clic en **Editor de administración de directivas de grupo**. El **seleccionar un objeto de directiva de grupo** abre el cuadro de diálogo.

     > [!IMPORTANT]
     > Asegúrese de seleccionar **Editor de administración de directivas de grupo** y no **Group Policy Management**. Si selecciona **Group Policy Management**, se producirá un error en la configuración mediante estas instrucciones y un certificado de servidor no se inscribe automáticamente para sus NPSs.

4. En **Objeto de directiva de grupo**, haga clic en **Examinar**. Se abre el cuadro de diálogo **Buscar un objeto de directiva de grupo**.
5. En **Dominios, unidades organizativas y objetos de directiva de grupo vinculados**, haga clic en **Directiva predeterminada de dominio** y, a continuación, haga clic en **Aceptar**.
6. Haga clic en **Finalizar**y, a continuación, en **Aceptar**.
7. Haga doble clic en **Directiva predeterminada de dominio**. En la consola, expanda la ruta siguiente: **Configuración del equipo**, **Directivas**, **Configuración de Windows**, **Configuración de seguridad** y, a continuación, **Directivas de clave pública**.
8. Haga clic en **Directivas de clave pública**. En el panel de detalles, haz doble clic en **Cliente de Servicios de servidor de certificados - Inscripción automática**. El **propiedades** abre el cuadro de diálogo. Configure los siguientes elementos y haga clic en **Aceptar**:

     1. En **Modelo de configuración**, seleccione **Habilitado**.
     2. Active la casilla **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados**.
     3. Active la casilla **Actualizar certificados que usan plantillas de certificado**.

9. Haga clic en **Aceptar**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurar la inscripción automática del certificado del usuario

1. En el equipo donde está instalado AD DS, abra Windows PowerShell&reg;, tipo **mmc**, y, a continuación, presione ENTRAR. Se abre Microsoft Management Console.
2. En el menú **Archivo**, haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.
3. En **complementos disponibles**, desplácese hacia abajo y haga doble clic en **Editor de administración de directivas de grupo**. El **seleccionar un objeto de directiva de grupo** abre el cuadro de diálogo.

     > [!IMPORTANT]
     > Asegúrese de seleccionar **Editor de administración de directivas de grupo** y no **Group Policy Management**. Si selecciona **Group Policy Management**, se producirá un error en la configuración mediante estas instrucciones y un certificado de servidor no se inscribe automáticamente para sus NPSs.

4. En **Objeto de directiva de grupo**, haga clic en **Examinar**. Se abre el cuadro de diálogo **Buscar un objeto de directiva de grupo**.
5. En **Dominios, unidades organizativas y objetos de directiva de grupo vinculados**, haga clic en **Directiva predeterminada de dominio** y, a continuación, haga clic en **Aceptar**.
6. Haga clic en **Finalizar**y, a continuación, en **Aceptar**.
7. Haga doble clic en **Directiva predeterminada de dominio**. En la consola, expanda la ruta siguiente: **Configuración de usuario**, **directivas**, **Windows configuración**, **configuración de seguridad**.
8. Haga clic en **Directivas de clave pública**. En el panel de detalles, haz doble clic en **Cliente de Servicios de servidor de certificados - Inscripción automática**. El **propiedades** abre el cuadro de diálogo. Configure los siguientes elementos y haga clic en **Aceptar**:

     1. En **Modelo de configuración**, seleccione **Habilitado**.
     2. Active la casilla **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados**.
     3. Active la casilla **Actualizar certificados que usan plantillas de certificado**.

9. Haga clic en **Aceptar**.

## <a name="next-steps"></a>Pasos siguientes

[Actualizar directiva de grupo](refresh-group-policy.md)
