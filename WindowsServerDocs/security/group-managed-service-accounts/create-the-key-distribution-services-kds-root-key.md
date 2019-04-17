---
title: "Crear la clave raíz de la distribución de claves servicios KDS"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 30075e56f3ca8e90a0655508efeacfcf2aaa0337
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Crear la clave raíz de la distribución de claves servicios KDS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI describen cómo crear una clave raíz de (kdssvc.dll) de servicio de distribución de claves de Microsoft en el controlador de dominio mediante Windows PowerShell para generar grupo cuenta de servicio administrada contraseñas en Windows Server 2012.

 Controladores de dominio de Windows Server 2012 (corriente continua) requieren una clave raíz para empezar a generar gMSA contraseñas. Los controladores de dominio esperará hasta 10 horas desde el momento de creación para permitir que todos los controladores de dominio convergen su replicación de AD antes de permitir la creación de un gMSA. Las 10 horas es una medida de seguridad para impedir la generación de contraseña antes de que todos los controladores de dominio en el entorno son capaces de responder a solicitudes de gMSA.  Si intentas usar un gMSA demasiado pronto la clave no es posible que se ha replicados a todos los controladores de Windows Server 2012 y, por tanto, la contraseña de recuperación puede producir un error cuando el host gMSA intenta recuperar la contraseña. También pueden producirse errores de recuperación de contraseña de gMSA al usar los controladores de dominio con programas de replicación limitada o si hay un problema de replicación.

Pertenencia a la **administradores de dominio** o **administradores** grupos o equivalente, es lo mínimo necesario para completar este procedimiento. Para obtener más información sobre el uso de las cuentas y adecuadas pertenencias a grupos, [Local y dominio predeterminada grupos](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Una arquitectura de 64 bits se necesita para ejecutar los comandos de Windows PowerShell que se usan para administrar las cuentas de servicio administradas de grupo.

#### <a name="to-create-the-kds-root-key-using-the-new-kdsrootkey-cmdlet"></a>Para crear la clave raíz KDS mediante el cmdlet New-KdsRootKey

1.  En el controlador de dominio de Windows Server 2012, ejecutar Windows PowerShell desde la barra de tareas.

2.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **Add-KdsRootKey - EffectiveImmediately**

    > [!TIP]
    > El parámetro de tiempo eficaz puede usarse para dar tiempo para las claves se propague a todos los controladores de dominio antes de su uso. Usar Add-KdsRootKey - EffectiveImmediately agregará una clave raíz en el destino de controlador de dominio que se usará el servicio KDS inmediatamente. Sin embargo, otros controladores de dominio de Windows Server 2012 no podrán usar la clave raíz hasta replicación es correcta.

Para entornos de prueba con solo un controlador de dominio, puedes crear una clave raíz KDS y establecer la hora de inicio en el pasado a evitar la espera de intervalo para generar claves mediante el siguiente procedimiento. Valida que un evento 4004 se ha registrado en el registro de eventos kds.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Para crear la clave raíz KDS en un entorno de prueba de la eficacia de inmediato

1.  En el controlador de dominio de Windows Server 2012, ejecutar Windows PowerShell desde la barra de tareas.

2.  En el símbolo del sistema para el módulo de Active Directory de Windows PowerShell, escribe los siguientes comandos y, a continuación, presiona ENTRAR:

    **$un = Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey - EffectiveTime $b**

    O bien usar un solo comando

    **((Get-date).addhours(-10)) Add-KdsRootKey - EffectiveTime**

## <a name="see-also"></a>Consulta también
[Introducción a grupo administra las cuentas de servicio](getting-started-with-group-managed-service-accounts.md)


