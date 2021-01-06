---
title: Configurar la inscripción automática de certificados de servidor
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: c81e85cb-ecb8-442a-ad27-442c2f9e40df
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 6f37d19cd1805b5c8cf0be267e3d3317f17fcd11
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947861"
---
# <a name="configure-certificate-auto-enrollment"></a>Configurar la inscripción automática de certificados

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!NOTE]
> Antes de realizar este procedimiento, debe configurar una plantilla de certificado de servidor mediante el uso del complemento Plantillas de certificado de Microsoft Management Console en una CA que ejecute AD CS.
El requisito mínimo para completar este procedimiento es la pertenencia al grupo **Administradores de empresas** y al grupo **Admins. del dominio** del dominio raíz.

## <a name="configure-server-certificate-auto-enrollment"></a>Configurar la inscripción automática de certificados de servidor

1. En el equipo en el que está instalado AD DS, abra Windows PowerShell &reg; , escriba **MMC** y, a continuación, presione Entrar. Se abre Microsoft Management Console.
2. En el menú **Archivo** , haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.
3. En **complementos disponibles**, desplácese hacia abajo y haga doble clic en **Editor de administración de directivas de grupo**. Se abre el cuadro de diálogo **Seleccionar objeto de directiva de grupo** .

     > [!IMPORTANT]
     > Asegúrese de seleccionar **Editor de administración de directivas de grupo** y no **Directiva de Grupo administración**. Si selecciona **Directiva de grupo Management**, se producirá un error en la configuración con estas instrucciones y no se realizará la inscripción automática de un certificado de servidor en su NPSs.

4. En **Objeto de directiva de grupo**, haga clic en **Examinar**. Se abre el cuadro de diálogo **Buscar un objeto de directiva de grupo**.
5. En **Dominios, unidades organizativas y objetos de directiva de grupo vinculados**, haga clic en **Directiva predeterminada de dominio** y, a continuación, haga clic en **Aceptar**.
6. Haz clic en **Finalizar** y, a continuación, en **Aceptar**.
7. Haga doble clic en **Directiva predeterminada de dominio**. En la consola de, expanda la siguiente ruta de acceso: **configuración del equipo**, **directivas**, **configuración de Windows**, configuración de **seguridad** y, a continuación, **directivas de clave pública**.
8. Haga clic en **Directivas de clave pública**. En el panel de detalles, haga doble clic en **Cliente de Servicios de servidor de certificados - Inscripción automática**. Se abrirá el cuadro de diálogo **propiedades** . Configure los siguientes elementos y haga clic en **Aceptar**:

     1. En **Modelo de configuración**, seleccione **Habilitado**.
     2. Active la casilla **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados**.
     3. Active la casilla **Actualizar certificados que usan plantillas de certificado**.

9. Haga clic en **Aceptar**.

## <a name="configure-user-certificate-auto-enrollment"></a>Configurar la inscripción automática de certificados de usuario

1. En el equipo en el que está instalado AD DS, abra Windows PowerShell &reg; , escriba **MMC** y, a continuación, presione Entrar. Se abre Microsoft Management Console.
2. En el menú **Archivo** , haga clic en **Agregar o quitar complemento**. Se abre el cuadro de diálogo **Agregar o quitar complementos**.
3. En **complementos disponibles**, desplácese hacia abajo y haga doble clic en **Editor de administración de directivas de grupo**. Se abre el cuadro de diálogo **Seleccionar objeto de directiva de grupo** .

     > [!IMPORTANT]
     > Asegúrese de seleccionar **Editor de administración de directivas de grupo** y no **Directiva de Grupo administración**. Si selecciona **Directiva de grupo Management**, se producirá un error en la configuración con estas instrucciones y no se realizará la inscripción automática de un certificado de servidor en su NPSs.

4. En **Objeto de directiva de grupo**, haga clic en **Examinar**. Se abre el cuadro de diálogo **Buscar un objeto de directiva de grupo**.
5. En **Dominios, unidades organizativas y objetos de directiva de grupo vinculados**, haga clic en **Directiva predeterminada de dominio** y, a continuación, haga clic en **Aceptar**.
6. Haz clic en **Finalizar** y, a continuación, en **Aceptar**.
7. Haga doble clic en **Directiva predeterminada de dominio**. En la consola de, expanda la siguiente ruta de acceso: **configuración de usuario**, **directivas**, **configuración de Windows**, configuración de **seguridad**.
8. Haga clic en **Directivas de clave pública**. En el panel de detalles, haga doble clic en **Cliente de Servicios de servidor de certificados - Inscripción automática**. Se abrirá el cuadro de diálogo **propiedades** . Configure los siguientes elementos y haga clic en **Aceptar**:

     1. En **Modelo de configuración**, seleccione **Habilitado**.
     2. Active la casilla **Renovar certificados expirados, actualizar certificados pendientes y quitar certificados revocados**.
     3. Active la casilla **Actualizar certificados que usan plantillas de certificado**.

9. Haga clic en **Aceptar**.

## <a name="next-steps"></a>Pasos siguientes

[Actualizar directiva de grupo](refresh-group-policy.md)
