---
title: "Configuración de la protección de LSA adicionales"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 038e7c2b-c032-491f-8727-6f3f01116ef9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fcfb0dab10d28413cf4ad06dd583274f217c91fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-additional-lsa-protection"></a>Configuración de la protección de LSA adicionales

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI se explica cómo configurar la protección adicional para el proceso de autoridad de seguridad Local (LSA) evitar la inserción de código que podría poner en peligro las credenciales.

LSA, que incluye el proceso de servicio de servidor de autoridad de seguridad Local (LSASS), valida los usuarios para inicios de sesión locales y remotos y aplica directivas de seguridad local. El sistema operativo de Windows 8.1 proporciona protección adicional para la LSA inyección de código no protegidas procesos y evitar que la memoria de lectura. Esto proporciona mayor seguridad de las credenciales que la LSA almacena y administra. Se pueden configurar la configuración de proceso protegido de LSA en Windows 8.1, pero no puede configurarse en Windows RT 8.1. Cuando esta configuración se usa en combinación con el arranque seguro, se logra una protección adicional porque deshabilitar la clave del registro HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa no tiene ningún efecto.

### <a name="protected-process-requirements-for-plug-ins-or-drivers"></a>Requisitos de proceso protegido para complementos o controladores
Para que un controlador o LSA complemento cargar correctamente como un proceso protegido, debe cumplir los siguientes criterios:

1.  Comprobación de firmas

    El modo protegido requiere que los complementos que se carga en la LSA está firmado digitalmente con una firma de Microsoft. Por lo tanto, los complementos que están firmados o no estén firmados con una firma de Microsoft no se cargará de LSA. Ejemplos de estos complementos son los controladores de tarjeta inteligente, complementos criptográficos y filtros de contraseña.

    Complementos LSA que son los controladores, como tarjetas inteligentes, se deben firmar mediante el uso de la certificación de WHQL. Para obtener más información, consulta [firma de versión de WHQL (controladores de Windows)](https://msdn.microsoft.com/library/windows/hardware/ff553976%28v=vs.85%29.aspx).

    Complementos LSA que no tienen un proceso de certificación de WHQL, deben estar firmados mediante el uso de la [servicio LSA de firma de archivos](https://go.microsoft.com/fwlink/?LinkId=392590).

2.  Cumplimiento de la orientación del proceso de ciclo de vida de desarrollo de seguridad de Microsoft (SDL)

    Todos los complementos deben cumplir con la orientación del proceso SDL aplicable. Para obtener más información, consulta el [apéndice del ciclo de vida de desarrollo de seguridad de Microsoft (SDL)](https://msdn.microsoft.com/library/windows/desktop/cc307891.aspx).

    Aunque los complementos están firmados correctamente con una firma de Microsoft, puede provocar incumplimiento con el proceso SDL error al cargar un complemento.

#### <a name="recommended-practices"></a>Procedimientos recomendados
Usa la lista siguiente para probar exhaustivamente esa protección LSA está habilitada antes de implementar la característica ampliamente:

-   Identificar todos los complementos LSA y controladores que están en uso dentro de la organización. 
    Esto incluye los controladores no son de Microsoft o complementos como controladores de tarjetas inteligentes y complementos criptográficos y cualquier desarrolladas internamente software que se usa para aplicar filtros de contraseña o las notificaciones de cambio de contraseña.

-   Asegúrate de que todos los complementos LSA se firman digitalmente con un certificado de Microsoft para que el complemento no no se cargará.

-   Asegúrate de que todos los complementos firmados correctamente pueden cargar correctamente en LSA y que funcionan según lo esperado.

-   Usar los registros de auditoría para identificar los complementos LSA y controladores que no se ejecuta como un proceso protegido.

## <a name="how-to-identify-lsa-plug-ins-and-drivers-that-fail-to-run-as-a-protected-process"></a>Cómo identificar los complementos LSA y controladores que no se ejecuta como un proceso protegido
Los eventos descritos en esta sección se encuentran en el funcionamiento del registro en aplicaciones y servicios Logs\Microsoft\Windows\CodeIntegrity. Puede ayudar a identificar los complementos LSA y controladores que no se pueden cargar debido a la firma motivos. Para administrar estos eventos, puedes usar la **wevtutil** herramienta de línea de comandos. Para obtener información acerca de esta herramienta, consulta [Wevtutil](../../administration/windows-commands/Wevtutil.md).

### <a name="before-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Si optas por antes de: cómo identificar los complementos y controladores cargados por el lsass.exe
Puedes usar el modo auditoría para identificar los complementos LSA y controladores que no se cargará en modo de protección de LSA. En el modo de auditoría, el sistema generará registros de eventos, identifica todos los complementos y controladores que no se cargará en LSA si se habilita la protección de LSA. Se registran los mensajes sin bloquear los controladores o los complementos.

##### <a name="to-enable-the-audit-mode-for-lsassexe-on-a-single-computer-by-editing-the-registry"></a>Para habilitar el modo auditoría para Lsass.exe en un solo equipo modificando el registro

1.  Abre el Editor del registro de (RegEdit.exe) y navegar a la clave del registro que se encuentra en: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image archivo ejecución Options\LSASS.exe.

2.  Establece el valor de la clave del registro **AuditLevel = DWORD: 00000008**.

3.  Reinicia el equipo.

Analizar los resultados del evento 3065 y evento 3066.

-   **Evento 3065**: este evento se registra la integridad de código comprobar determina que intentó cargar un controlador determinado que no cumplían los requisitos de seguridad para las secciones compartidas de un proceso (normalmente lsass.exe). Sin embargo, debido a la directiva del sistema que está establecida, se permitió la imagen para cargar.

-   **Evento 3066**: este evento se registra la integridad de código comprobar determina que un proceso (normalmente lsass.exe) intentó cargar un controlador determinado que no cumplían lo requisitos de nivel de firma de Microsoft. Sin embargo, debido a la directiva del sistema que está establecida, se permitió la imagen para cargar.

> [!IMPORTANT]
> Estos eventos operativos no se generan cuando un depurador de kernel está conectado y habilitado en un sistema.
> 
> Si un complemento o controlador contiene secciones compartidas, se registra el evento 3066 con eventos 3065. Quitar las secciones compartidas debe impedir que ambos eventos que se producen a menos que el complemento no cumple lo requisitos de nivel de firma de Microsoft.

Para habilitar el modo de auditoría para varios equipos en un dominio, puedes usar la extensión del lado cliente del registro para la directiva de grupo para distribuir el valor del registro de nivel de auditoría de Lsass.exe. Debes modificar la clave del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image archivo ejecución Options\LSASS.exe.

##### <a name="to-create-the-auditlevel-value-setting-in-a-gpo"></a>Para crear el valor AuditLevel en un GPO

1.  Abre la consola de administración de directivas de grupo (GPMC).

2.  Crear una nueva directiva objeto grupo (GPO) que está vinculada en el nivel de dominio o que está vinculado a la unidad organizativa que contiene tus cuentas de equipo. O bien, puedes seleccionar un GPO que ya se ha implementado.

3.  Haz clic en el GPO y, a continuación, haz clic en **editar** para abrir el Editor de administración de directivas de grupo.

4.  Expande **configuración del equipo**, expanda **preferencias**y después expande **configuración de Windows**.

5.  Haz clic en **registro**, elija **nueva**y, a continuación, haz clic en **elemento del registro**. La **nuevas propiedades del registro** aparece el cuadro de diálogo.

6.  En la **subárbol** la lista, haz clic en **HKEY_LOCAL_MACHINE.**

7.  En la **ruta de acceso Key** la lista, busca **SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image archivo ejecución Options\LSASS.exe**.

8.  En la **nombre de valor**, escriba **AuditLevel**.

9. En la **tipo de valor** cuadro, haga clic para seleccionar la **REG_DWORD**.

10. En la **información del valor**, escriba **00000008**.

11. Haz clic en **Aceptar**.

> [!NOTE]
> Para el GPO surta efecto, el cambio de GPO se debe replicar en todos los controladores de dominio del dominio.

Para participar de protección adicional de LSA en varios equipos, puedes usar la extensión del lado cliente del registro para la directiva de grupo mediante la modificación HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa. Para conocer los pasos necesarios hacer esto, vea [cómo configurar una protección de credenciales LSA adicional](#BKMK_HowToConfigure) en este tema.

### <a name="after-opting-in-how-to-identify-plug-ins-and-drivers-loaded-by-the-lsassexe"></a>Después Si optas por: cómo identificar los complementos y controladores cargados por el lsass.exe
Puedes usar el registro de eventos para identificar los complementos LSA y controladores que no se pudo cargar en modo de protección de LSA. Cuando se habilita el proceso de LSA protegido, el sistema genera los registros de eventos que identificar todos los complementos y controladores que no se pudo cargar en LSA.

Analizar los resultados de 3033 eventos y eventos 3063.

-   **Evento 3033**: este evento se registra la integridad de código comprobar determina que un proceso (normalmente lsass.exe) intentó cargar un controlador que no cumplían lo requisitos de nivel de firma de Microsoft.

-   **Evento 3063**: este evento se registra la integridad de código comprobar determina que intentó cargar un controlador que no cumplían los requisitos de seguridad para las secciones compartidas de un proceso (normalmente lsass.exe).

Secciones compartidas suelen ser el resultado de técnicas de programación que permiten interactuar con otros procesos que usan el mismo contexto de seguridad de los datos de instancia. Esto puede crear vulnerabilidades de seguridad.

## <a name="BKMK_HowToConfigure"></a>Cómo configurar la protección de LSA adicional de credenciales
En dispositivos que ejecutan Windows 8.1 (con o sin el arranque seguro o UEFI), es posible de configuración siguiendo los procedimientos descritos en esta sección. Para los dispositivos que ejecutan Windows RT 8.1, protección frente a lsass.exe siempre está habilitado y no se podrá desactivar.

### <a name="on-x86-based-or-x64-based-devices-using-secure-boot-and-uefi-or-not"></a>En los dispositivos basados en x86 o x64 64 con UEFI y arranque seguro o no
En los dispositivos basados en x86 o x64 64 que usan el arranque seguro y UEFI, una variable UEFI está establecida en el firmware UEFI cuando protección de LSA se habilita mediante la clave del registro. Cuando la configuración se almacena en el firmware, la variable UEFI no pueden eliminarse ni cambiar la clave del registro. La variable UEFI debe restablecerse.

Se deshabilitan basado en x86 o x64 64 dispositivos que no son compatibles con UEFI o el arranque seguro, no puedes almacenar la configuración de protección de LSA en el firmware y confíe únicamente en la presencia de la clave del registro. En este escenario, es posible deshabilitar la protección de LSA mediante el acceso remoto al dispositivo.

Puedes usar los siguientes procedimientos para habilitar o deshabilitar la protección de LSA:

##### <a name="to-enable-lsa-protection-on-a-single-computer"></a>Para habilitar la protección de LSA en un solo equipo

1.  Abre el Editor del registro de (RegEdit.exe) y navegar a la clave del registro que se encuentra en: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Establece el valor de la clave del registro: "RunAsPPL" = dword: 00000001.

3.  Reinicia el equipo.

##### <a name="to-enable-lsa-protection-using-group-policy"></a>Para habilitar la protección de LSA mediante Directiva de grupo

1.  Abre la consola de administración de directivas de grupo (GPMC).

2.  Crear un nuevo GPO que está vinculada en el nivel de dominio o que está vinculado a la unidad organizativa que contiene tus cuentas de equipo. O bien, puedes seleccionar un GPO que ya se ha implementado.

3.  Haz clic en el GPO y, a continuación, haz clic en **editar** para abrir el Editor de administración de directivas de grupo.

4.  Expande **configuración del equipo**, expanda **preferencias**y después expande **configuración de Windows**.

5.  Haz clic en **registro**, elija **nueva**y, a continuación, haz clic en **elemento del registro**. La **nuevas propiedades del registro** aparece el cuadro de diálogo.

6.  En la **subárbol** la lista, haz clic en **HKEY_LOCAL_MACHINE**.

7.  En la **ruta de acceso Key** la lista, busca **SYSTEM\CurrentControlSet\Control\Lsa**.

8.  En la **nombre de valor**, escriba **RunAsPPL**.

9. En la **tipo de valor** cuadro, haz clic en el **REG_DWORD**.

10. En la **información del valor**, escriba **00000001**.

11. Haz clic en **Aceptar**.

##### <a name="to-disable-lsa-protection"></a>Para deshabilitar la protección de LSA

1.  Abre el Editor del registro de (RegEdit.exe) y navegar a la clave del registro que se encuentra en: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa.

2.  Eliminar el siguiente valor de la clave del registro: "RunAsPPL" = dword: 00000001.

3.  Usar la herramienta de exclusión de proceso protegido autoridad de seguridad Local (LSA) para eliminar la variable UEFI si el dispositivo está usando el arranque seguro.

    Para obtener más información sobre la herramienta de exclusión, consulta [autoridad de seguridad Local (LSA) Descargar proceso protegido exclusión desde el centro de descarga de Microsoft oficiales](https://www.microsoft.com/download/details.aspx?id=40897).

    Para obtener más información sobre la administración de arranque seguro, consulta [Firmware UEFI](https://technet.microsoft.com/library/hh824898.aspx).

    > [!WARNING]
    > Cuando el arranque seguro está desactivado, se restablecen todas las configuraciones relacionadas con UEFI y arranque seguro. Deberías desactivar el arranque seguro solo cuando no se hayan podido todos los otros medios, para desactivar la protección de LSA.

### <a name="verifying-lsa-protection"></a>Comprobar la protección de LSA
Para detectar si se inició LSA en el modo protegido cuando Windows inicia, busca el siguiente evento de WinInit en la **sistema** el registro en **registros de Windows**:

-   12: LSASS.exe se inició como un proceso protegido con nivel: 4

## <a name="additional-resources"></a>Recursos adicionales
[Administración y protección de credenciales](credentials-protection-and-management.md)

[Servicio de firmas de LSA del archivo](https://go.microsoft.com/fwlink/?LinkId=392590)


