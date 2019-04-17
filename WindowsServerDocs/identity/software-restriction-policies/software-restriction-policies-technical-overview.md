---
title: "Introducción técnica de las directivas de restricción de software"
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d007d55ced9c6a18581eaedb4edb66db9eeccab9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies-technical-overview"></a>Introducción técnica de las directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describen las directivas de restricción de software, cuándo y cómo usar la característica, ¿qué cambios se han implementado en las versiones anteriores y proporciona vínculos a recursos adicionales que te ayudarán a crear e implementar las directivas de restricción de software a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software proporcionan a los administradores con un mecanismo controlada por la directiva de grupo para identificar el software y controlar su capacidad para ejecutarse en el equipo local. Estas directivas pueden usarse para proteger los equipos que ejecutan sistemas operativos de Microsoft Windows (a partir de Windows Server 2003 y Windows XP Professional) conflictos conocidos y proteger los equipos frente a amenazas de seguridad como malintencionados virus y troyanos. También puedes usar las directivas de restricción de software para crear una configuración muy restringida para equipos, en los que permites solo específicamente identificadas aplicaciones que se ejecutan. Las directivas de restricción de software están integradas con la directiva de grupo y Microsoft Active Directory. También puedes crear las directivas de restricción de software en equipos independientes.

Las directivas de restricción de software son las directivas de confianza, que son normativa establecida por un administrador para restringir los scripts y otros códigos que no es de plena confianza de ejecución. La extensión de directivas de restricción de Software en el Editor de directivas de grupo Local proporciona una única interfaz de usuario a través del cual se puede administrar la configuración para restringir el uso de aplicaciones en el equipo local o en todo un dominio.

## <a name="procedures"></a>Procedimientos

-   [Administrar las directivas de restricción de Software](administer-software-restriction-policies.md)

    -   [Determinar permitir y denegar lista y un inventario de la aplicación para las directivas de restricción de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Trabajar con reglas de directivas de restricción de Software](work-with-software-restriction-policies-rules.md)

    -   [Usar las directivas de restricción de Software para ayudar a proteger el equipo contra los Virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Solucionar problemas de las directivas de restricción de Software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Escenarios de uso de directivas de restricción de software
Los usuarios empresariales colaborarán mediante correo electrónico, mensajería instantánea y las aplicaciones de punto a punto. A medida que estas colaboraciones aumentan, especialmente con el uso de Internet en informática empresarial, también lo hacen las amenazas contra código malintencionado, como los gusanos, virus y un usuario malintencionado o amenazas atacante.

Los usuarios pueden recibir el código hostil de muchas formas, que van desde nativo Windows archivos ejecutables (.exe), en las macros de documentos (como archivos .doc), para las secuencias de comandos (como archivos de vbs). Atacantes o usuarios malintencionados suelen usan métodos de ingeniería social para conseguir que los usuarios ejecutar código que contengan virus y gusanos. (Ingeniería Social es un término para engañar a personas para que revelen sus contraseñas o algún tipo de información de seguridad). Si este código está activado, puede generar los ataques de denegación de servicio en la red, enviar datos con información confidencial o privadas a Internet, correr el riesgo la seguridad del equipo o pueden dañar el contenido de la unidad de disco duro.

Los usuarios y las organizaciones de TI deben ser capaces de determinar qué software es seguro ejecutar y cuál no. Con la gran cantidad y formularios que pueda tomar código hostil, esto se convierte en una tarea difícil.

Para ayudar a proteger sus equipos de red de código hostil y software desconocido o no compatible, las organizaciones pueden implementar las directivas de restricción de software como parte de su estrategia global de seguridad.

Los administradores pueden usar las directivas de restricción de software para las siguientes tareas:

-   Definir qué es el código de confianza

-   Diseñar una directiva de grupo flexible para regular scripts, archivos ejecutables y los controles ActiveX

Las directivas de restricción de software se aplican por el sistema operativo y las aplicaciones (como aplicaciones de scripts) que cumplan con las directivas de restricción de software.

En concreto, los administradores pueden usar las directivas de restricción de software para los fines siguientes:

-   Especificar qué software (archivos ejecutables) se puede ejecutar en los equipos cliente

-   Impedir que los usuarios ejecuten programas específicos en equipos compartidos

-   Especifica quién puede agregar editores de confianza a los equipos cliente

-   Establecer el ámbito de las directivas de restricción de software (especificar si las directivas afectan a todos los usuarios o un subconjunto de usuarios en los equipos cliente)

-   Impedir que se ejecuten en el equipo local, unidad organizativa (OU), sitio o dominio archivos ejecutables. Esto sería adecuado en los casos cuando no estás usando las directivas de restricción de software para solucionar posibles problemas con los usuarios malintencionados.

## <a name="BKMK_Diffs_Changes"></a>Las diferencias y cambios en la funcionalidad
No hay ningún cambio en la funcionalidad de SRP para Windows Server 2012 y Windows 8.

**Versiones compatibles**

Las directivas de restricción de software pueden solo configuradas en y aplicar a equipos que ejecutan al menos Windows Server 2003, incluidos Windows Server 2012 y al menos Windows XP, incluido Windows 8.

> [!NOTE]
> Determinadas ediciones de la Windows a partir de sistema operativo cliente con Windows Vista no tienen directivas de restricción de Software. Equipos que no se administra en un dominio mediante la directiva de grupo no pueden recibir directivas distribuidas.

**Comparar las funciones de control de aplicación de directivas de restricción de Software y AppLocker**

La siguiente tabla compara las características y funciones de la característica de directivas de restricción de Software (SRP) y AppLocker.

|Función de control de la aplicación|SRP|AppLocker|
|----------------|----|-------|
|Ámbito|Las directivas de SRP pueden aplicarse a todos los sistemas operativos de Windows a partir de Windows XP y Windows Server 2003.|Las directivas de AppLocker se aplican solo a Windows Server 2008 R2, Windows Server 2012, Windows 7 y Windows 8.|
|Creación de directivas|Las directivas de SRP se mantienen a través de la directiva de grupo y solo el administrador del GPO puede actualizar la directiva SRP. El administrador en el equipo local puede modificar las directivas SRP definidas en el GPO local.|Las directivas de AppLocker se mantienen a través de la directiva de grupo y solo el administrador del GPO puede actualizar la directiva. El administrador en el equipo local puede modificar las directivas de AppLocker definidas en el GPO local.<br /><br />AppLocker permite la personalización de mensajes de error para dirigir a los usuarios a una página Web para obtener ayuda.|
|Mantenimiento de directivas|Las directivas de SRP deben actualizarse mediante el complemento de directiva de seguridad Local (si se crean las directivas localmente) o la consola de administración de directivas de grupo (GPMC).|Las directivas de AppLocker pueden actualizarse mediante la directiva de seguridad Local complemento (si se crean las directivas localmente), o la GPMC o los cmdlets de Windows PowerShell AppLocker.|
|Aplicación de la directiva|Las directivas de SRP se distribuyen a través de la directiva de grupo.|Las directivas de AppLocker se distribuyen a través de la directiva de grupo.|
|Modo de aplicación|SRP funciona en el "modo Denegar lista" donde los administradores pueden crear reglas para los archivos que no desean permitir en la empresa, mientras que el resto del archivo se pueden ejecutar de manera predeterminada.<br /><br />SRP también pueden configurarse en el "modo Permitir lista" tal que el de manera predeterminada se bloquean todos los archivos y los administradores necesitan crear reglas de permiso para archivos que quieren permitir.|AppLocker funciona de forma predeterminada en el "modo Permitir lista" donde solo esos archivos se pueden ejecutar para el que allí es una coincidencia de regla de permiso.|
|Tipos de archivo que se pueden controlar|SRP puede controlar los tipos de archivo siguientes:<br /><br />-Archivos ejecutables<br />-Dlls<br />: Scripts<br />: Instaladores de Windows<br /><br />SRP no puede controlar cada tipo de archivo por separado. Todas las reglas SRP están en una colección de regla única.|AppLocker puede controlar los tipos de archivo siguientes:<br /><br />-Archivos ejecutables<br />-Dlls<br />: Scripts<br />: Instaladores de Windows<br />-Empaquetado de aplicaciones e instaladores (Windows Server 2012 y Windows 8)<br /><br />AppLocker mantiene una colección de reglas independientes para cada uno de los cinco tipos de archivo.|
|Tipos de archivo designados|SRP admite una lista extensible de tipos de archivo que se consideran ejecutables. Los administradores pueden agregar extensiones a los archivos que deben considerarse ejecutables.|AppLocker no admite esto. AppLocker admite actualmente las siguientes extensiones de archivo:<br /><br />-Archivos ejecutables (.exe, .com)<br />-DLL (.ocx, .dll)<br />-Scripts (.vbs, .js,. ps1, .cmd y .bat)<br />-Windows Installer (.msi, .mst y .msp)<br />-Los instaladores de aplicaciones (.appx)|
|Tipos de reglas|SRP admite cuatro tipos de reglas:<br /><br />-Hash<br />-Path<br />: Firma<br />: Zona de Internet|AppLocker admite tres tipos de reglas:<br /><br />-Hash<br />-Path<br />-Editor|
|Editar el valor de hash|SRP permite a los administradores proporcionar valores de hash personalizados.|AppLocker calcula el valor de hash sí mismo. Usa internamente el hash SHA1 Authenticode para archivos ejecutables portables (Exe y Dll) y los instaladores de Windows y un hash de archivo plano SHA1 para el resto.|
|Compatibilidad con diferentes niveles de seguridad|Con SRP, los administradores pueden especificar los permisos con los que puede ejecutar una aplicación. Por lo tanto, un administrador puede configurar una regla de manera que el Bloc de notas siempre se ejecuta con permisos restringidos y nunca con privilegios administrativos.<br /><br />En Windows Vista y versiones anteriores de SRP admite varios niveles de seguridad. En Windows 7, esa lista se restringió a tan solo dos niveles: no permitido y sin restricciones (usuario básico se traduce en no permitido).|AppLocker no admite niveles de seguridad.|
|Administrar aplicaciones empaquetadas e instaladores de aplicaciones empaquetadas|No se puede|.AppX es un tipo de archivo válido que puede administrar AppLocker.|
|Selección del destino una regla a un usuario o un grupo de usuarios|Reglas SRP se aplican a todos los usuarios en un equipo determinado.|Las reglas de AppLocker pueden destinarse a un usuario específico o un grupo de usuarios.|
|Compatibilidad con las excepciones de regla|SRP no admite excepciones de regla|Las reglas de AppLocker pueden tener las excepciones que permiten a los administradores crear reglas como "Permitir todo desde Windows excepto Regedit.exe".|
|Compatibilidad con el modo auditoría|SRP no admite el modo auditoría. La única manera de probar las directivas de SRP es configurar un entorno de prueba y ejecutar algunos experimentos.|AppLocker admite el modo de auditoría que permite a los administradores probar el efecto de la directiva en el entorno de producción real sin afectar a la experiencia del usuario. Una vez que estés satisfecho con los resultados, puede iniciar la aplicación de la directiva.|
|Compatibilidad para exportar e importar directivas|SRP no admite la importación y exportación de directivas.|AppLocker admite la importación y exportación de directivas. Esto te permite crear directivas de AppLocker en un equipo de muestra, probarla y, a continuación, exportar esa directiva y vuelva a importarlo al GPO deseado.|
|Aplicación de reglas|Internamente, el cumplimiento de las reglas SRP se realiza en el modo de usuario que es menos seguro.|De forma interna, se aplican las reglas de AppLocker para archivos exe y DLL en modo kernel, que es más seguro que aplicarlas en el modo de usuario.|

## <a name="system-requirements"></a>Requisitos del sistema
Las directivas de restricción de software pueden solo configuradas en y aplicar a equipos que ejecutan al menos Windows Server 2003 y al menos Windows XP. Directiva de grupo es necesaria para distribuir objetos de directiva de grupo que contienen las directivas de restricción de software.

## <a name="software-restriction-policies-components-and-architecture"></a>Arquitectura y componentes de las directivas de restricción de software
Las directivas de restricción de software proporcionan un mecanismo para el sistema operativo y las aplicaciones compatibles con las directivas de restricción de software para restringir el tiempo de ejecución de programas de software.

En un nivel alto, las directivas de restricción de software consisten en los siguientes componentes:

-   Directivas de restricción de software API. Las Interfaces de programación de aplicaciones (API) se usan para crear y configurar las reglas que constituyen la directiva de restricción de software. También hay API para la realización de consultas, el procesamiento de directivas de restricción de software y ejecución de las directivas de restricción de software.

-   Una herramienta de administración directivas de restricción de software. Se trata de la **las directivas de restricción de Software** extensión de la **Editor de objetos de directiva de grupo Local** complemento, que los administradores usan para crear y modificar las directivas de restricción de software.

-   Un conjunto de API del sistema operativo y las aplicaciones que llamen a las directivas de restricción de software API para proporcionar la aplicación de las directivas de restricción de software en tiempo de ejecución.

-   Directiva de grupo y Active Directory. Las directivas de restricción de software dependen de la infraestructura de directiva de grupo para propagar las directivas de restricción de software de Active Directory a los clientes adecuados y para el ámbito y filtrado de la aplicación de estas directivas a los equipos de destino apropiado.

-   Authenticode y WinVerify confiar en las API que se usan para procesar los archivos ejecutables firmados.

-   Visor de eventos. Las funciones que usa eventos de registro de directivas de restricción de software en los registros del Visor de eventos.

-   Conjunto resultante de directivas (RSoP), que puede ayudar a diagnosticar de la directiva eficaz que se aplicará a un cliente.

Para obtener más información sobre la arquitectura SRP, cómo SRP administra las reglas, procesos e interacciones, consulta [cómo Software trabajo de directivas de restricción](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) en la biblioteca técnica de Windows Server 2003.

## <a name="BKMK_Best_Practices"></a>Procedimientos recomendados

### <a name="do-not-modify-the-default-domain-policy"></a>No modifiques la directiva de dominio predeterminada.

-   Si no se modifica la directiva de dominio predeterminada, siempre tendrá la opción de volver a aplicar la directiva de dominio predeterminada si algo va mal con la directiva de dominio personalizado.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Crear un objeto de directiva de grupo independiente para las directivas de restricción de software.

-   Si creas un objeto de directiva de grupo (GPO) independiente para las directivas de restricción de software, puedes deshabilitar las directivas de restricción de software en caso de emergencia sin deshabilitar el resto de la directiva de dominio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Si tienes problemas con la configuración de directiva aplicada, reinicie Windows en modo seguro.

-   Las directivas de restricción de software no se aplican cuando se inicia Windows en modo seguro. Si accidentalmente bloquear una estación de trabajo con las directivas de restricción de software, reinicie el equipo en modo seguro, iniciar sesión como administrador local, modificar la directiva, ejecutar **gpupdate**, reinicia el equipo y, a continuación, inicie sesión normalmente.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Ten cuidado al definir una configuración predeterminada de no permitido.

-   Al definir una configuración predeterminada de **no permitido**, todo el software está permitido excepto el software que se ha permitido explícitamente. Cualquier archivo que quieres abrir debe tener una restricción de software las directivas de la regla que le permite abrir.

-   Para proteger los administradores de bloqueo a sí mismos fuera del sistema, cuando se establece el nivel de seguridad de forma predeterminada en **no permitido**, cuatro reglas de ruta de acceso del registro se crean automáticamente. Puedes eliminar o modificar estas reglas de ruta de acceso del registro; Sin embargo, esto no se recomienda.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Para mayor seguridad, usar listas de control de acceso junto con las directivas de restricción de software.

-   Los usuarios pueden intentar sortear las directivas de restricción de software al cambiar el nombre o mover los archivos no permitidos o sobrescribir archivos no restringidos. Como resultado, se recomienda usar listas de control de acceso (ACL) para denegar a los usuarios el acceso necesario para realizar estas tareas.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Probar completamente nuevas configuraciones de directiva en entornos de prueba antes de aplicar la configuración de directiva a tu dominio.

-   Nuevas configuraciones de directiva podrían actuar de forma diferente de lo esperado originalmente. Las pruebas reducen la probabilidad de que se produzcan problemas al implementar la configuración de directiva en la red.

-   Puedes configurar un dominio de prueba, independiente del dominio de la organización, en el que se va a probar la nueva configuración de directiva. También puedes probar la configuración de directiva mediante la creación de un GPO de prueba y vincula a una unidad organizativa de prueba. Cuando pruebes la configuración de directiva con usuarios de prueba, puedes vincular el GPO de prueba con el dominio.

-   No establezcas programas o archivos a **no permitido** sin pruebas para ver lo que puede ser el efecto. Restricciones en determinados archivos en serio pueden afectar al funcionamiento de su equipo o red.

-   La información que se han escrito incorrectamente o errores de escritura pueden provocar una configuración de directiva no se realiza según lo esperado. Prueba la nueva configuración de directiva antes de aplicarlas puede evitar que un comportamiento inesperado.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrar la configuración de directiva de usuario en función de pertenencia a grupos de seguridad.

-   Puedes especificar usuarios o grupos que no desea una configuración de directiva para aplicar desactivando el **aplicar directiva de grupo** y **lectura** casillas de verificación que se encuentran en la **seguridad** ficha del cuadro de diálogo Propiedades para el GPO.

-   Cuando se deniega el permiso de lectura, la configuración de directiva no se descarga el equipo. Como resultado, se consume menos ancho de banda, descarga la configuración de directiva innecesarias, lo que permite a la red funcione más rápidamente. Para denegar el permiso de lectura, selecciona **denegar** para el **lectura** casilla de verificación que se encuentra en la **seguridad** ficha del cuadro de diálogo Propiedades para el GPO.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>No se vincula a un GPO en otro dominio o sitio.

-   Vinculación a un GPO en otro dominio o sitio puede provocar un rendimiento deficiente.

## <a name="additional-resources"></a>Recursos adicionales

|Tipo de contenido|Referencias|
|--------|-------|
|**Planeación**|[Referencia técnica de las directivas de restricción de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Operaciones**|[Administrar las directivas de restricción de Software](administer-software-restriction-policies.md)|
|**Solución de problemas**|[Directivas de restricción de software (2003) de la solución de problemas](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Seguridad**|[Directivas de amenazas y contramedidas de restricción de Software (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Directivas de amenazas y contramedidas de restricción de Software (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Herramientas y opciones de configuración**|[Las directivas de restricción de software herramientas y opciones de configuración (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Recursos de la Comunidad**|[Bloqueo de la aplicación con las directivas de restricción de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


