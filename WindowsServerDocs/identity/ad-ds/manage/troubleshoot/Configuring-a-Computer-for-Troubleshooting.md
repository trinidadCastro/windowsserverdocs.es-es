---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configuración de un equipo para la solución de problemas
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1acb5f7d309d58ed4a5a3aca6bb89f01c0cbf933
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854126"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configuración de un equipo para la solución de problemas

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de usar técnicas de solución de problemas avanzadas para identificar y corregir problemas de Active Directory, configurar los equipos para solucionar problemas. También debe tener un conocimiento básico de conceptos, procedimientos y herramientas de solución de problemas.

Para obtener información acerca de las herramientas de supervisión para Windows Server, consulte la Guía paso a paso para [supervisión del rendimiento y confiabilidad en Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>Tareas de configuración para la solución de problemas

Para configurar el equipo para solucionar problemas de servicios de dominio de Active Directory (AD DS), realice las siguientes tareas:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Instalar herramientas de administración remota del servidor para AD DS

Cuando se instala AD DS para crear un controlador de dominio, las herramientas administrativas que usan para administrar AD DS se instalan automáticamente. Si desea administrar los controladores de dominio de forma remota desde un equipo que no sea un controlador de dominio, puede instalar las herramientas de administración de servidor (RSAT) remoto en un servidor miembro o estación de trabajo que se está ejecutando una versión compatible de Windows. RSAT reemplaza las herramientas de soporte técnico de Windows desde Windows Server 2003.

Para obtener información acerca de cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurar el Monitor de confiabilidad y rendimiento

Windows Server incluye la confiabilidad de Windows y el Monitor de rendimiento, que es un complemento Microsoft Management Console (MMC) que combina la funcionalidad de herramientas independientes anteriores, incluidos los registros de rendimiento y alertas, Server Performance Advisor, y Monitor de sistema. Este complemento proporciona una interfaz gráfica de usuario (GUI) para personalizar conjuntos de recopiladores de datos y sesiones de seguimiento de eventos.

Monitor de confiabilidad y rendimiento también incluye el Monitor de confiabilidad, un complemento de MMC que realiza el seguimiento de cambios en el sistema y los compara con los cambios en la estabilidad del sistema, que proporciona una vista gráfica de su relación.

### <a name="set-logging-levels"></a>Establecer niveles de registro

Si la información que recibe en el registro del servicio de directorio en el Visor de eventos no es suficiente para solucionar problemas, elevar los niveles de registro mediante el uso de la entrada del Registro adecuados en **HKEY_LOCAL_ MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**.

De forma predeterminada, los niveles de registro para todas las entradas se establecen en **0**, que proporciona la cantidad mínima de información. Es el más alto nivel de registro **5**. Aumentar el nivel de una entrada hace que se registrarán en el registro de eventos del servicio de directorio eventos adicionales.

Utilice el procedimiento siguiente para cambiar el nivel de registro para una entrada de diagnóstico. Para completar este procedimiento, se requiere como mínimo la pertenencia a **Admins. del dominio** o equivalente.

> [!WARNING]
> Te recomendamos que no modifiques directamente el registro a menos que no haya ninguna otra alternativa. Las modificaciones del registro no se validan mediante el editor del registro o mediante Windows antes de que se aplican, y como resultado, se pueden almacenar valores incorrectos. Esto puede dar lugar a errores irrecuperables en el sistema. Cuando sea posible, usar Directiva de grupo u otras herramientas de Windows, como los complementos MMC para realizar tareas, en lugar de modificar directamente el registro. Si tienes que modificar el registro, ten mucha precaución.
>

Para cambiar el nivel de registro para una entrada de diagnóstico

1. Haga clic en **iniciar** > **ejecutar** > tipo **regedit** > haga clic en **Aceptar**.
2. Navegue a la entrada para el que desea establecer el registro.
   * EJEMPLO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Haga doble clic en la entrada y en **Base**, haga clic en **Decimal**.
4. En **valor**, escriba un entero de **0** a través de **5**y, a continuación, haga clic en **Aceptar**.
