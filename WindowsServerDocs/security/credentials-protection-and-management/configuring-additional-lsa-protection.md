---
title: Configuración de protección LSA adicional
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 54bc100c935df2ff0cc7086b258fb395458f259f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638080"
---
# <a name="configuring-additional-lsa-protection"></a>Configuración de protección LSA adicional

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema dedicado a los profesionales de TI se explica cómo configurar la protección adicional para el proceso de autoridad de seguridad local (LSA) de cara a evitar una inserción de código que podría poner en peligro las credenciales.

El proceso LSA, que incluye el proceso del Servicio de servidor de autoridad de seguridad local (LSASS), valida a los usuarios para los inicios de sesión locales y remotos y exige la aplicación de directivas de seguridad local. El sistema operativo Windows 8.1 proporciona protección adicional para LSA para evitar la lectura de memoria y la inserción de código por parte de procesos no protegidos. Esto proporciona seguridad adicional para las credenciales que LSA almacena y administra. La configuración del proceso protegido para LSA puede configurarse en Windows 8.1, pero no puede configurarse en Windows RT 8,1. Cuando esta configuración se utiliza conjuntamente con el arranque seguro, la protección adicional se consigue porque deshabilitar la clave del Registro HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa no tiene ninguna consecuencia.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Requisitos de acceso protegido para complementos o controladores
Para que un complemento o un controlador de LSA se cargue correctamente como proceso protegido, debe cumplir los criterios siguientes:

1.  Verificación de firmas

    El modo protegido requiere que todos los complementos que se carguen en LSA estén firmados digitalmente mediante una firma de Microsoft. Por consiguiente, todos los complementos no firmados o que no tengan una firma de Microsoft no se cargarán en LSA. Algunos ejemplos de estos complementos son los controladores de tarjetas inteligentes, los complementos de cifrado y los filtros de contraseña.

    Los complementos de LSA que sean controladores, como los controladores de tarjetas inteligentes, tienen que estar firmados con la certificación de WHQL. Para obtener más información, vea [firma de versión de WHQL](/windows-hardware/drivers/install/whql-release-signature).

    Los complementos de LSA que no tengan un proceso de certificación de WHQL deben estar firmados con el [servicio de firma de archivos para LSA](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Adhesión a la orientación del proceso de ciclo de vida de desarrollo de seguridad (SDL) de Microsoft

    Todos los complementos deben estar en conformidad con la orientación del proceso SDL aplicable. Para obtener más información, consulte el [apéndice del ciclo de vida de desarrollo de seguridad (SDL) de Microsoft](/previous-versions/windows/desktop/cc307891(v=msdn.10)).

    Incluso aunque los complementos estén debidamente firmados con una firma de Microsoft, la no conformidad con el proceso SDL puede generar errores al cargar un complemento.

#### <a name="recommended-practices"></a>Procedimientos recomendados
Utiliza la lista siguiente para probar exhaustivamente que la protección LSA esté habilitada antes de implementar la característica de forma generalizada:

-   Identifica todos los complementos y controladores de LSA que se están utilizando dentro de la organización.
    Esto incluye los controladores o complementos que no son de Microsoft, como los controladores de tarjetas inteligentes, los complementos de cifrado y cualquier software desarrollado internamente que se utilice para exigir la aplicación de los filtros de contraseña o las notificaciones de cambio de contraseña.

-   Asegúrate de que todos los complementos de LSA estén firmados digitalmente con un certificado de Microsoft para que no falle la carga del complemento.

-   Asegúrate de que todos los complementos firmados correctamente puedan cargarse adecuadamente en LSA y que tengan el rendimiento esperado.

-   Utiliza los registros de auditoría para identificar los complementos y controladores de LSA que no se hayan ejecutado correctamente como proceso protegido.

#### <a name="limitations-introduced-with-enabled-lsa-protection"></a>Limitaciones introducidas con la protección LSA habilitada

Si la protección LSA está habilitada, no se puede depurar un complemento LSA personalizado.
No se puede adjuntar un depurador a LSASS cuando se trata de un proceso protegido.
En general, no hay ninguna manera compatible de depurar un proceso protegido en ejecución.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Cómo identificar los complementos y controladores de LSA que no se hayan ejecutado correctamente como proceso protegido
Los eventos descritos en esta sección están ubicados en el registro operativo, bajo Registros de aplicaciones y servicios\Microsoft\Windows\CodeIntegrity. Pueden ayudarte a identificar los complementos y controladores de LSA que no se han cargado correctamente debido a problemas relacionados con firmas. Para administrar estos eventos, utiliza la herramienta de línea de comandos **wevtutil**. Para obtener más información sobre esta herramienta, consulta [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Antes de participar: Cómo identificar los complementos y controladores cargados por lsass.exe
Puedes utilizar el modo de auditoría para identificar los complementos y controladores de LSA que no se cargarán correctamente en modo de protección LSA. Mientras esté en modo de auditoría, el sistema generará registros de eventos e identificará todos los complementos y controladores que no se cargarán correctamente bajo LSA si se ha habilitado la protección LSA. Los mensajes se registran sin bloquear los complementos o controladores.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Para habilitar el modo de auditoría para Lsass.exe en un único equipo mediante la edición del Registro

1.  Abra el Editor del Registro (RegEdit.exe) y navegue hasta la clave del Registro ubicada en: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

2.  Establece el valor de la clave del Registro en **AuditLevel=dword:00000008**.

3.  Reinicie el equipo.

Analiza los resultados del evento 3065 y el evento 3066.

Después, puede ver estos eventos en Visor de eventos: Microsoft-Windows-Codeintegrity/Operational:

-   **Evento 3065**: este evento registra que una comprobación de integridad de código ha determinado que un proceso (normalmente lsass.exe) ha intentado cargar un controlador específico que no ha cumplido los requisitos de seguridad para las secciones compartidas. No obstante, a causa de la directiva del sistema establecida, se ha permitido la carga de la imagen.

-   **Evento 3066**: este evento registra que una comprobación de integridad de código ha determinado que un proceso (normalmente lsass.exe) ha intentado cargar un controlador específico que no ha cumplido los requisitos de nivel de firma de Microsoft. No obstante, a causa de la directiva del sistema establecida, se ha permitido la carga de la imagen.

> [!IMPORTANT]
> Estos eventos operativos no se generan cuando un depurador de kernel se adjunta y se habilita en un sistema.
>
> Si un complemento o un controlador contiene secciones compartidas, el evento 3066 se registra con el evento 3065. La eliminación de las secciones compartidas evitará que se produzcan ambos eventos, a menos que el complemento no cumpla los requisitos de nivel de firma de Microsoft.

Para habilitar el modo de auditoría para varios equipos en un dominio, utiliza la extensión del lado cliente de directiva de grupo del Registro para implementar el valor del Registro de nivel de auditoría Lsass.exe. Debes modificar la clave del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe.

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Para crear la configuración del valor AuditLevel en un GPO

1.  Abre la Consola de administración de directivas de grupo (GPMC).

2.  Crea un nuevo objeto de directiva de grupo (GPO) que esté vinculado en el nivel de dominio o a la unidad organizativa que contiene las cuentas de tu equipo. O puedes seleccionar un GPO que ya esté implementado.

3.  Haz clic con el botón derecho en el GPO y luego haz clic en **Editar** para abrir el Editor de administración de directivas de grupo.

4.  Expande **Configuración del equipo**, expande **Preferencias** y, a continuación, expande **Configuración de Windows**.

5.  Haz clic con el botón derecho en **Registro**, selecciona **Nuevo** y, a continuación, haz clic en **Elemento del Registro**. Aparece el cuadro de diálogo **Nuevas propiedades de Registro**.

6.  En la lista **subárbol** , haga clic en **HKEY_LOCAL_MACHINE.**

7.  En la lista **Ruta de la clave**, busca **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\LSASS.exe**.

8.  En el cuadro **Nombre del valor**, escribe **AuditLevel**.

9. En el cuadro **Tipo de valor**, haz clic para seleccionar **REG_DWORD**.

10. En el cuadro **Datos del valor**, escribe **00000008**.

11. Haga clic en **OK**.

> [!NOTE]
> Para que se aplique el GPO, el cambio del GPO debe replicarse a todos los controladores de dominio en el dominio.

Para participar en la protección LSA adicional en varios equipos, utiliza la extensión del lado cliente de directiva de grupo del Registro modificando HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa. Para conocer los pasos necesarios para llevarlo a cabo, consulta la información acerca de [cómo configurar la protección LSA adicional de credenciales](#BKMK_HowToConfigure) en este tema.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Tras la participación: Cómo identificar los complementos y controladores cargados por lsass.exe
Puedes utilizar el registro de eventos para identificar los complementos y controladores de LSA que no se han cargado correctamente en modo de protección LSA. Cuando el proceso protegido de LSA está habilitado, el sistema genera registros de eventos que identifican todos los complementos y controladores que no se han podido cargar bajo LSA.

Analiza los resultados del evento 3033 y el evento 3063.

Después, puede ver estos eventos en Visor de eventos: Microsoft-Windows-Codeintegrity/Operational:

-   **Evento 3033**: este evento registra que una comprobación de integridad de código ha determinado que un proceso (normalmente lsass.exe) ha intentado cargar un controlador que no ha cumplido los requisitos de nivel de firma de Microsoft.

-   **Evento 3063**: este evento registra que una comprobación de integridad de código ha determinado que un proceso (normalmente lsass.exe) ha intentado cargar un controlador que no ha cumplido los requisitos de seguridad para las secciones compartidas.

Las secciones compartidas normalmente son el resultado de técnicas de programación que permiten que los datos de instancia interactúen con otros procesos que utilizan el mismo contexto de seguridad. Esto puede crear vulnerabilidades de seguridad.

## <a name="how-to-configure-additional-lsa-protection-of-credentials"></a><a name="BKMK_HowToConfigure"></a>Cómo configurar la protección LSA adicional de credenciales
En los dispositivos que ejecutan Windows 8.1 (con o sin arranque seguro o UEFI), la configuración es posible mediante la realización de los procedimientos descritos en esta sección. En el caso de los dispositivos que ejecutan Windows RT 8,1, la protección de lsass.exe está siempre habilitada y no se puede desactivar.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>En los dispositivos basados en x86 o x64 con o sin arranque seguro y UEFI
En los dispositivos basados en x86 o x64 que usan arranque seguro o UEFI, se establece una variable UEFI en el firmware UEFI cuando la protección LSA está habilitada mediante la clave del registro. Cuando la configuración se almacena en el firmware, la variable UEFI no puede suprimirse ni cambiarse en la clave del Registro. La variable UEFI debe restablecerse.

Los dispositivos basados en x86 o x64 que no admiten UEFI o en los que el arranque seguro está deshabilitado no pueden almacenar la configuración para la protección LSA en el firmware y únicamente dependen de la presencia de la clave del Registro. En este escenario, es posible deshabilitar la protección LSA con el acceso remoto al dispositivo.

Puedes utilizar los procedimientos siguientes para habilitar o deshabilitar la protección LSA:

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Para habilitar la protección LSA en un único equipo

1.  Abra el Editor del Registro (RegEdit.exe) y desplácese a la clave del Registro que se encuentra en: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Establezca el valor de la clave del Registro en: "RunAsPPL"=dword:00000001.

3.  Reinicie el equipo.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Para habilitar la protección LSA con la directiva de grupo

1.  Abre la Consola de administración de directivas de grupo (GPMC).

2.  Crea un nuevo GPO que esté vinculado a la unidad organizativa que contenga las cuentas de tu equipo. O puedes seleccionar un GPO que ya esté implementado.

3.  Haz clic con el botón derecho en el GPO y luego haz clic en **Editar** para abrir el Editor de administración de directivas de grupo.

4.  Expande **Configuración del equipo**, expande **Preferencias** y, a continuación, expande **Configuración de Windows**.

5.  Haz clic con el botón derecho en **Registro**, selecciona **Nuevo** y, a continuación, haz clic en **Elemento del Registro**. Aparece el cuadro de diálogo **Nuevas propiedades de Registro**.

6.  En la lista **Subárbol**, haz clic en **HKEY_LOCAL_MACHINE**.

7.  En la lista **ruta de acceso** de la clave, vaya a **System\CurrentControlSet\Control\Lsa**.

8.  En el cuadro **nombre de valor** , escriba **RunAsPPL**.

9. En el cuadro **Tipo de valor**, haz clic en **REG_DWORD**.

10. En el cuadro **información del valor** , escriba **00000001**.

11. Haga clic en **OK**.

##### <a name="to-disable-lsa-protection"></a>Para deshabilitar la protección LSA

1.  Abra el Editor del Registro (RegEdit.exe) y desplácese a la clave del Registro que se encuentra en: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Elimine el valor siguiente de la clave del Registro: "RunAsPPL"=dword:00000001.

3.  Utiliza la herramienta de cancelación de procesos protegidos de autoridad de seguridad local (LSA) para suprimir la variable UEFI si el dispositivo está utilizando el arranque de seguridad.

    Para obtener más información acerca de la herramienta de cancelación, consulte la información acerca de la [descarga de la herramienta de cancelación de procesos protegidos de autoridad de seguridad local (LSA) desde el Centro de descarga de Microsoft oficial](https://www.microsoft.com/download/details.aspx?id=40897).

    Para obtener más información acerca de la administración del arranque de seguridad, consulte [Firmware UEFI](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824898(v=win.10)).

    > [!WARNING]
    > Cuando el arranque seguro está desactivado, todas las configuraciones relacionadas con el arranque seguro y UEFI se restablecen. Debes desactivar el arranque seguro únicamente cuando hayan fallado todos los demás medios de deshabilitación de la protección LSA.

### <a name="verifying-lsa-protection"></a>Verificar la protección LSA
Para averiguar si LSA se ha iniciado en modo protegido cuando se ha iniciado Windows, busca el evento WinInit siguiente en el registro **Sistema** bajo **Registros de Windows**:

-   12: LSASS.exe se inició como un proceso protegido con nivel: 4

## <a name="additional-resources"></a>Recursos adicionales
[Protección y administración de credenciales](credentials-protection-and-management.md)

[Servicio de firma de archivos para LSA](https://go.microsoft.com/fwlink/?LinkId=392590)