---
title: Crear la clave raíz del Servicio de distribución de claves (KDS)
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
ms.openlocfilehash: 3d5f7b46b28e6a2fbfafb664b69aebc8d34886fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867216"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>Crear la clave raíz del Servicio de distribución de claves (KDS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema para profesionales de TI describe cómo crear una clave de raíz del servicio de distribución de claves (kdssvc.dll) de Microsoft en el controlador de dominio mediante Windows PowerShell para generar contraseñas de cuenta de servicio administrada de grupo en Windows Server 2012.

 Controladores de dominio (DC) de Windows Server 2012 requieren una clave raíz para comenzar a generar contraseñas gMSA. Antes de permitir la creación de una gMSA, todos los controladores de dominio esperarán hasta 10 horas desde el momento de la creación para que converjan sus replicaciones de AD. Las 10 horas son una medida de seguridad para evitar que la generación de contraseñas se produzca antes de que todos los controladores de dominio del entorno sean capaces de responder a las peticiones de gMSA.  Si intenta usar una gMSA demasiado pronto la clave podría no haberse replicada en todos los DC de Windows Server 2012 y, por tanto, la recuperación de contraseñas puede producir un error cuando el host de gMSA intente recuperar la contraseña. Los errores de recuperación de contraseña de gMSA también pueden ocurrir si se usan controladores de dominio con programaciones de replicación limitadas o si hay un problema de replicación.

Para poder realizar este procedimiento, es necesario, como mínimo, pertenecer a **Admins. del dominio**, **Administradores de empresas** o a otro grupo equivalente. Para ver información detallada sobre el uso de las cuentas adecuadas y las pertenencias a grupos, consulte [Grupos predeterminados locales y de dominio](https://technet.microsoft.com/library/dd728026(WS.10).aspx).

> [!NOTE]
> Se necesita una arquitectura de 64 bits para ejecutar los comandos de Windows PowerShell que se usan para administrar cuentas de servicio administradas de grupo.

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>Para crear la clave raíz KDS con el cmdlet Add-KdsRootKey

1.  En el controlador de dominio de Windows Server 2012, ejecute Windows PowerShell desde la barra de tareas.

2.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **Add-KdsRootKey -EffectiveImmediately**

    > [!TIP]
    > El parámetro Effective time se puede usar para dar a las claves tiempo para que se propaguen a todos los controladores de dominio antes de su uso. Con Add-KdsRootKey - EffectiveImmediately agregará una clave raíz para el controlador de dominio que será utilizado por el servicio KDS inmediatamente de destino. Sin embargo, otros controladores de dominio de Windows Server 2012 no podrá usar la clave raíz hasta que la replicación sea correcta.

Para los entornos de prueba con solo un controlador de dominio, puedes usar el procedimiento siguiente para crear una clave raíz de KDS y establecer la hora de inicio en el pasado para evitar el intervalo de espera. Comprueba que se haya registrado un evento 4004 en el registro de eventos de KDS.

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>Para crear la clave raíz de KDS en un entorno de pruebas para que sea efectivo de forma inmediata

1.  En el controlador de dominio de Windows Server 2012, ejecute Windows PowerShell desde la barra de tareas.

2.  Escribe los siguientes comandos en el símbolo del sistema del módulo de Active Directory de Windows PowerShell y, luego, presiona ENTRAR:

    **$a=Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey -EffectiveTime $b**

    O usa un solo comando

    **Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>Vea también
[Introducción a grupo de cuentas de servicio administradas](getting-started-with-group-managed-service-accounts.md)


