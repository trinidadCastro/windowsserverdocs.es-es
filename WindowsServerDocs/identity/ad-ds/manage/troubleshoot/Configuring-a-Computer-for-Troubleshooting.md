---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configuración de un equipo para la solución de problemas
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a71e1b92962ae9904262367f2c2697ecaa206ed8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965937"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configuración de un equipo para la solución de problemas

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de usar técnicas avanzadas de solución de problemas para identificar y solucionar problemas Active Directory, configure los equipos para solucionar problemas. También debe tener un conocimiento básico de los conceptos, los procedimientos y las herramientas de solución de problemas.

Para obtener información acerca de las herramientas de supervisión para Windows Server, consulte la guía paso a paso para la [supervisión de rendimiento y confiabilidad en Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737) .

## <a name="configuration-tasks-for-troubleshooting"></a>Tareas de configuración para la solución de problemas

Para configurar el equipo para la solución de problemas Active Directory Domain Services (AD DS), realice las siguientes tareas:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Instalación de Herramientas de administración remota del servidor para AD DS

Al instalar AD DS para crear un controlador de dominio, se instalan automáticamente las herramientas administrativas que se usan para administrar AD DS. Si desea administrar los controladores de dominio de forma remota desde un equipo que no sea un controlador de dominio, puede instalar el Herramientas de administración remota del servidor (RSAT) en un servidor miembro o estación de trabajo que ejecute una versión compatible de Windows. RSAT reemplaza las herramientas de soporte de Windows de Windows Server 2003.

Para obtener información sobre cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](../../../../remote/remote-server-administration-tools.md).

### <a name="configure-reliability-and-performance-monitor"></a>Configurar el monitor de confiabilidad y rendimiento

Windows Server incluye el monitor de confiabilidad y rendimiento de Windows, que es un complemento de Microsoft Management Console (MMC) que combina la funcionalidad de herramientas independientes anteriores, como Registros y alertas de rendimiento, el asesor de rendimiento de servidor y el monitor de sistema. Este complemento proporciona una interfaz gráfica de usuario (GUI) para personalizar conjuntos de recopiladores de datos y sesiones de seguimiento de eventos.

El monitor de confiabilidad y rendimiento también incluye el monitor de confiabilidad, un complemento MMC que realiza un seguimiento de los cambios en el sistema y los compara con los cambios en la estabilidad del sistema, lo que proporciona una vista gráfica de su relación.

### <a name="set-logging-levels"></a>Establecimiento de los niveles de registro

Si la información que recibe en el registro del servicio de directorio Visor de eventos no es suficiente para solucionar problemas, aumente los niveles de registro mediante la entrada del registro adecuada en **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\ntds\diagnostics**.

De forma predeterminada, los niveles de registro para todas las entradas se establecen en **0**, lo que proporciona la cantidad mínima de información. El nivel de registro más alto es **5**. Al aumentar el nivel de una entrada, se registran eventos adicionales en el registro de eventos del servicio de directorio.

Utilice el procedimiento siguiente para cambiar el nivel de registro de una entrada de diagnóstico. La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

> [!WARNING]
> Te recomendamos que no modifiques directamente el registro a menos que no haya ninguna otra alternativa. El editor del registro o Windows no validan las modificaciones del registro antes de que se apliquen y, como resultado, se pueden almacenar valores incorrectos. Esto puede dar lugar a errores irrecuperables en el sistema. Cuando sea posible, use directiva de grupo u otras herramientas de Windows, como los complementos de MMC, para llevar a cabo tareas, en lugar de editar el registro directamente. Si tienes que modificar el registro, ten mucha precaución.
>

Para cambiar el nivel de registro de una entrada de diagnóstico

1. Haga clic en **iniciar**  >  **Ejecutar** > escriba **regedit** > haga clic en **Aceptar**.
2. Desplácese a la entrada para la que desea establecer el inicio de sesión.
   * EJEMPLO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Haga doble clic en la entrada y, en **base**, haga clic en **decimal**.
4. En **valor**, escriba un número entero comprendido entre **0** y **5**y, a continuación, haga clic en **Aceptar**.
