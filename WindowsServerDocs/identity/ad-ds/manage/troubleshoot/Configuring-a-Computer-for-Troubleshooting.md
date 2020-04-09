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
ms.openlocfilehash: d9d279615dc1f70ffdcff9e49a4aa619f0106a93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822978"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configuración de un equipo para la solución de problemas

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de usar técnicas avanzadas de solución de problemas para identificar y solucionar problemas Active Directory, configure los equipos para solucionar problemas. También debe tener un conocimiento básico de los conceptos, los procedimientos y las herramientas de solución de problemas.

Para obtener información acerca de las herramientas de supervisión para Windows Server, consulte la guía paso a paso para la [supervisión de rendimiento y confiabilidad en Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737) .

## <a name="configuration-tasks-for-troubleshooting"></a>Tareas de configuración para la solución de problemas

Para configurar el equipo para la solución de problemas Active Directory Domain Services (AD DS), realice las siguientes tareas:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Instalación de Herramientas de administración remota del servidor para AD DS

Al instalar AD DS para crear un controlador de dominio, se instalan automáticamente las herramientas administrativas que se usan para administrar AD DS. Si desea administrar los controladores de dominio de forma remota desde un equipo que no sea un controlador de dominio, puede instalar el Herramientas de administración remota del servidor (RSAT) en un servidor miembro o estación de trabajo que ejecute una versión compatible de Windows. RSAT reemplaza las herramientas de soporte de Windows de Windows Server 2003.

Para obtener información sobre cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurar el monitor de confiabilidad y rendimiento

Windows Server incluye el monitor de confiabilidad y rendimiento de Windows, que es un complemento de Microsoft Management Console (MMC) que combina la funcionalidad de herramientas independientes anteriores, como Registros y alertas de rendimiento, el asesor de rendimiento de servidor y el monitor de sistema. Este complemento proporciona una interfaz gráfica de usuario (GUI) para personalizar conjuntos de recopiladores de datos y sesiones de seguimiento de eventos.

El monitor de confiabilidad y rendimiento también incluye el monitor de confiabilidad, un complemento MMC que realiza un seguimiento de los cambios en el sistema y los compara con los cambios en la estabilidad del sistema, lo que proporciona una vista gráfica de su relación.

### <a name="set-logging-levels"></a>Establecer los niveles de registro

Si la información que recibe en el registro del servicio de directorio Visor de eventos no es suficiente para solucionar problemas, aumente los niveles de registro mediante la entrada del registro adecuada en **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\ntds\diagnostics**.

De forma predeterminada, los niveles de registro para todas las entradas se establecen en **0**, lo que proporciona la cantidad mínima de información. El nivel de registro más alto es **5**. Al aumentar el nivel de una entrada, se registran eventos adicionales en el registro de eventos del servicio de directorio.

Utilice el procedimiento siguiente para cambiar el nivel de registro de una entrada de diagnóstico. La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

> [!WARNING]
> Te recomendamos que no modifiques directamente el registro a menos que no haya ninguna otra alternativa. Ni el editor del Registro ni Windows validan las modificaciones del Registro antes de que se apliquen. Como resultado, se pueden almacenar valores incorrectos. Esto puede dar lugar a errores irrecuperables en el sistema. Cuando sea posible, use directiva de grupo u otras herramientas de Windows, como los complementos de MMC, para llevar a cabo tareas, en lugar de editar el registro directamente. Si tienes que modificar el registro, ten mucha precaución.
>

Para cambiar el nivel de registro de una entrada de diagnóstico

1. Haga clic en **inicio** > **Ejecutar** > escriba **regedit** > haga clic en **Aceptar**.
2. Desplácese a la entrada para la que desea establecer el inicio de sesión.
   * EJEMPLO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Haga doble clic en la entrada y, en **base**, haga clic en **decimal**.
4. En **valor**, escriba un número entero comprendido entre **0** y **5**y, a continuación, haga clic en **Aceptar**.
