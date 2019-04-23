---
title: Información técnica de Directivas de restricción de software
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830856"
---
# <a name="software-restriction-policies-technical-overview"></a>Información técnica de Directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe las directivas de restricción de software, cuándo y cómo usar la característica, qué cambios se han implementado en las versiones anteriores y proporciona vínculos a recursos adicionales que le ayudarán a crear e implementar directivas de restricción de software a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software ofrecen a los administradores un mecanismo controlado por directivas de grupo para identificar el software y controlar su capacidad para ejecutarse en el equipo local. Estas directivas pueden utilizarse para proteger los equipos que ejecutan sistemas operativos de Microsoft Windows (a partir de Windows Server 2003 y Windows XP Professional) de conflictos conocidos y proteger los equipos frente a amenazas de seguridad, como virus maliciosos y programas caballo de Troya. También puede usar las directivas de restricción de software para crear una configuración altamente restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Las directivas de restricción de software están integradas en Microsoft Active Directory y la directiva de grupo. También puede crear directivas de restricción de software en equipos independientes.

Las directivas de restricción de software son directivas de confianza, las cuáles son normativas establecidas por un administrador para restringir los scripts y otros códigos que no son de confianza plena. La extensión de directivas de restricción de Software para el Editor de directivas de grupo Local proporciona una interfaz de usuario único a través del cual se puede administrar la configuración para restringir el uso de aplicaciones en el equipo local o a lo largo de un dominio.

## <a name="procedures"></a>Procedimientos

-   [Administrar las directivas de restricción de Software](administer-software-restriction-policies.md)

    -   [Determinar la lista denegaciones y del inventario de aplicaciones para las directivas de restricción de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Trabajar con reglas de directivas de restricción de Software](work-with-software-restriction-policies-rules.md)

    -   [Utilizar directivas de restricción de Software para ayudar a proteger su equipo contra Virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Solucionar problemas de directivas de restricción de Software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Escenarios de uso de directivas de restricción de software
Los usuarios empresariales colaboran mediante correo electrónico, mensajería instantánea y las aplicaciones punto a punto. A medida que aumentan estas colaboraciones, especialmente con el uso de Internet en la informática empresarial, también lo hacen las amenazas de código malintencionado, como gusanos, virus y un usuario malintencionado o amenazas del atacante.

Los usuarios pueden recibir el código malintencionado en muchas formas, comprendido entre nativo Windows archivos ejecutables (.exe), en las macros en documentos (como archivos .doc), secuencias de comandos (como archivos .vbs). Los atacantes o usuarios malintencionados suelen utilizar métodos de ingeniería social para hacer que los usuarios ejecutar código que contiene virus y gusanos. (La ingeniería Social es un término para engañar a personas para que revelen sus contraseñas o algún tipo de información de seguridad). Si dicho código está activado, puede generar los ataques de denegación de servicio en la red, enviar datos privados o confidenciales a Internet, la seguridad del equipo correr el riesgo o dañar el contenido de la unidad de disco duro.

Los usuarios y organizaciones de TI deben ser capaces de determinar qué software es seguro para ejecutarse y que no sea. Con los formularios que puede tomar el código malintencionado y números grandes, esto se convierte en una tarea difícil.

Para ayudar a proteger sus equipos de red desde el código malintencionado y software desconocido o no compatible, las organizaciones pueden implementar directivas de restricción de software como parte de su estrategia de seguridad global.

Los administradores pueden usar las directivas de restricción de software para realizar las siguientes tareas:

-   Definir el código de confianza

-   Diseñar una directiva de grupo flexible para regular scripts, archivos ejecutables y controles ActiveX

El sistema operativo y las aplicaciones (como las aplicaciones de scripting) que cumplen con las directivas de restricción de software aplican las directivas de restricción de software.

Concretamente, los administradores pueden usar las directivas de restricción de software con los siguientes propósitos:

-   Especificar qué software (archivos ejecutables) puede ejecutar en los equipos cliente

-   Evitar que los usuarios ejecuten programas específicos en equipos compartidos.

-   Especifique quién puede agregar editores de confianza en los equipos cliente

-   Establecer el ámbito de las directivas de restricción de software (especificar si las directivas afectan a todos los usuarios o un subconjunto de usuarios en los equipos cliente)

-   Evitar que los archivos ejecutables se ejecuten en el equipo local, la unidad organizativa (OU), el sitio o el dominio. Esto resultaría apropiado cuando no está usando directivas de restricción de software para tratar problemas potenciales con usuarios malintencionados.

## <a name="BKMK_Diffs_Changes"></a>Las diferencias y cambios de funcionalidad
No hay ningún cambio en la funcionalidad de SRP para Windows Server 2012 y Windows 8.

**Versiones admitidas**

Las directivas de restricción de software solo se pueden configurar en y aplicarse a equipos con al menos Windows Server 2003, incluido Windows Server 2012 y al menos Windows XP, incluidos Windows 8.

> [!NOTE]
> Ciertas ediciones del principio de sistema operativo de cliente de Windows con Windows Vista no tiene directivas de restricción de Software. No se administra en un dominio mediante la directiva de grupo de equipos que no reciba directivas distribuidas.

**Comparación de las funciones de control de aplicación de directivas de restricción de Software y AppLocker**

En la tabla siguiente se comparan las características y funciones de la característica Directivas de restricción de software (SRP) y AppLocker.

|Función de control de aplicaciones|SRP|AppLocker|
|----------------|----|-------|
|Ámbito|Las directivas de SRP se pueden aplicar a todos los sistemas operativos Windows a partir de Windows XP y Windows Server 2003.|Las directivas de AppLocker se aplican solo a Windows Server 2008 R2, Windows Server 2012, Windows 7 y Windows 8.|
|Creación de directivas|Las directivas de SRP se mantienen a través de la directiva de grupo y solo el administrador del GPO puede actualizar la directiva de SRP. El administrador del equipo local puede modificar las directivas de SRP definidas en el GPO local.|Las directivas de AppLocker se mantienen a través de la directiva de grupo y solo el administrador del GPO puede actualizar la directiva. El administrador del equipo local puede modificar las directivas de AppLocker definidas en el GPO local.<br /><br />AppLocker permite la personalización de mensajes de error con el fin de dirigir a los usuarios a una página web para obtener ayuda.|
|Mantenimiento de directivas|Las directivas de SRP deben actualizarse mediante el complemento de directiva de seguridad local (si se crean las directivas localmente) o la consola de administración de directiva de grupo (GPMC).|Las directivas de AppLocker pueden actualizarse mediante el complemento de directiva de seguridad local (si se crean las directivas localmente), los GPMC o los cmdlets de Windows PowerShell AppLocker.|
|Aplicación de la directiva|Las directivas de SRP se distribuyen a través de la directiva de grupo.|Las directivas de AppLocker se distribuyen a través de la directiva de grupo.|
|Modo de cumplimiento|SRP funciona en el "modo de lista de denegación" donde los administradores pueden crear reglas para los archivos que no desea permitir en la empresa, mientras que el resto del archivo se pueden ejecutar de forma predeterminada.<br /><br />SRP también se puede configurar en el "modo de lista Permitir" que el de forma predeterminada todos los archivos están bloqueados y los administradores necesitan crear reglas de permiso para archivos que va a permitir.|AppLocker por funciona de forma predeterminada en el "permitir el modo de lista" donde solo esos archivos se pueden ejecutar para el que existe es una coincidencia de regla de permiso.|
|Tipos de archivo que se pueden controlar|SRP puede controlar los siguientes tipos de archivo:<br /><br />: Archivos ejecutables<br />-DLL<br />: Las secuencias de comandos<br />: Instaladores de Windows<br /><br />SRP no puede controlar cada tipo de archivo por separado. Todas las reglas de SRP están en una colección de regla única.|AppLocker puede controlar los siguientes tipos de archivo:<br /><br />: Archivos ejecutables<br />-DLL<br />: Las secuencias de comandos<br />: Instaladores de Windows<br />-Empaqueta aplicaciones e instaladores (Windows Server 2012 y Windows 8)<br /><br />AppLocker mantiene una colección de reglas independientes para cada uno de los cinco tipos de archivo.|
|Tipos de archivo designados|SRP admite una lista extensible de tipos de archivo que se consideran ejecutables. Los administradores pueden agregar extensiones a los archivos que deben considerarse ejecutables.|AppLocker no admite esto. AppLocker admite actualmente las siguientes extensiones de archivo:<br /><br />-Archivos ejecutables (.exe, .com)<br />-DLL (OCX, DLL)<br />-Las secuencias de comandos (.vbs, .js,. ps1, .cmd, .bat)<br />-Instaladores de Windows (.msi, .mst, .msp)<br />-Instaladores de aplicaciones empaquetadas (AppX)|
|Tipos de regla|SRP admite cuatro tipos de reglas:<br /><br />-Hash<br />: Ruta de acceso<br />-Signature<br />: Zona de Internet|AppLocker admite tres tipos de reglas:<br /><br />-Hash<br />: Ruta de acceso<br />: Publicador|
|Editar el valor de hash|SRP permite a los administradores proporcionar los valores hash personalizado.|AppLocker calcula el valor de hash por su cuenta. Internamente usa el hash SHA1 Authenticode para archivos ejecutables portables (Exe y Dll) y los instaladores de Windows y un hash de archivo sin formato SHA1 para el resto.|
|Compatibilidad con diferentes niveles de seguridad|Con SRP, los administradores pueden especificar los permisos con la que se puede ejecutar una aplicación. Por lo tanto, un administrador puede configurar una regla que el Bloc de notas siempre se ejecuta con permisos restringidos y nunca con privilegios administrativos.<br /><br />SRP en Windows Vista y varios niveles de seguridad anteriores compatibles. En Windows 7 esa lista se restringe a solo dos niveles: No permitido y sin restricciones (básica del usuario se traduce en no permitido).|AppLocker no admite niveles de seguridad.|
|Administrar aplicaciones empaquetadas e instaladores de aplicaciones empaquetadas|No se puede|.appx es un tipo de archivo válido que puede administrar AppLocker.|
|Selección del destino de una regla para un usuario o un grupo de usuarios|Las reglas de SRP se aplican a todos los usuarios en un equipo determinado.|Las reglas de AppLocker pueden destinarse a un usuario específico o un grupo de usuarios.|
|Compatibilidad con las excepciones de regla|SRP no admite excepciones de reglas.|Las reglas de AppLocker pueden tener las excepciones que permiten a los administradores crear reglas, como "Permitir todo el contenido de Windows, excepto Regedit.exe".|
|Compatibilidad con el modo auditoría|SRP no admite el modo auditoría. La única manera de probar las directivas de SRP es configurar un entorno de prueba y ejecutar algunos experimentos.|AppLocker admite el modo de auditoría, que permite a los administradores probar el efecto de la directiva en el entorno de producción real sin afectar a la experiencia del usuario. Una vez que estés satisfecho con los resultados, puedes iniciar la aplicación de la directiva.|
|Compatibilidad para exportar e importar directivas|SRP no admite la importación o la exportación de directivas.|AppLocker admite la importación y la exportación de directivas. Esto te permite crear la directiva de AppLocker en un equipo de muestra, probarla, exportar esa directiva y volver a importarla al GPO deseado.|
|Aplicación de reglas|De forma interna, el cumplimiento de las reglas de SRP se realiza en el modo de usuario, que es menos seguro.|Internamente, se aplican las reglas de AppLocker para archivos exe y DLL en el modo de núcleo que es más seguro que aplicarlos en el modo de usuario.|

## <a name="system-requirements"></a>Requisitos del sistema
Las directivas de restricción de software solo se pueden configurar en y aplicarse a equipos con al menos Windows Server 2003 y al menos Windows XP. Se requiere la directiva de grupo para distribuir los objetos de directiva de grupo que contiene las directivas de restricción de software.

## <a name="software-restriction-policies-components-and-architecture"></a>Arquitectura y componentes de las directivas de restricción de software
Las directivas de restricción de software proporcionan un mecanismo para el sistema operativo y aplicaciones compatibles con las directivas de restricción de software para restringir la ejecución en tiempo de ejecución de programas de software.

En un nivel alto, las directivas de restricción de software se componen de los siguientes componentes:

-   Directivas de restricción de software API. Las Interfaces de programación de aplicaciones (API) se usan para crear y configurar las reglas que constituyen la directiva de restricción de software. También hay API para consultar, procesamiento de directivas de restricción de software y aplicar las directivas de restricción de software.

-   Una herramienta de administración de directivas de restricción de software. Esto consiste el **Software Restriction Policies** extensión de la **Editor de objetos de directiva de grupo Local** complemento, que los administradores usar para crear y editar las directivas de restricción de software.

-   Un conjunto de API del sistema operativo y las aplicaciones que llaman a las directivas de restricción de software las API para proporcionar una aplicación de las directivas de restricción de software en tiempo de ejecución.

-   Active Directory y directiva de grupo. Las directivas de restricción de software dependen de la infraestructura de directiva de grupo para propagar las directivas de restricción de software de Active Directory a los clientes adecuados y para el ámbito y filtrado de la aplicación de estas directivas a adecuado equipos de destino.

-   Authenticode y WinVerify confiar en las API que se usan para procesar los archivos ejecutables firmados.

-   Visor de eventos. Las funciones usadas por eventos de registro de las directivas de restricción de software en los registros del Visor de eventos.

-   Conjunto resultante de directivas (RSoP), que pueden ayudar en el diagnóstico de la directiva efectiva que se aplicarán a un cliente.

Para obtener más información sobre la arquitectura SRP, consulte cómo SRP administra reglas, procesos e interacciones, [cómo Software funcionan de las directivas de restricción](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) en la biblioteca técnica de Windows Server 2003.

## <a name="BKMK_Best_Practices"></a>Procedimientos recomendados

### <a name="do-not-modify-the-default-domain-policy"></a>No modifique la directiva de dominio predeterminada.

-   Si no se modifica la directiva predeterminada de dominio, siempre tiene la opción de volver a aplicar la directiva predeterminada de dominio si algo va mal con la directiva de dominio personalizado.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Crear un objeto de directiva de grupo independiente para las directivas de restricción de software.

-   Si crea un objeto de directiva de grupo (GPO) independiente para las directivas de restricción de software, puede deshabilitar las directivas de restricción de software en caso de emergencia sin deshabilitar el resto de la directiva de dominio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Si experimenta problemas con la configuración de directiva aplicada, reinicie Windows en modo seguro.

-   Las directivas de restricción de software no se aplican cuando Windows se inicia en modo seguro. Si accidentalmente bloquea una estación de trabajo con directivas de restricción de software, reinicie el equipo en modo seguro, inicie sesión como administrador local, modifique la directiva, ejecutar **gpupdate**, reinicie el equipo y, a continuación, inicie sesión con normalidad.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Tenga cuidado al definir un valor predeterminado de no permitido.

-   Al definir un valor predeterminado de **no permitido**, todo el software está permitido excepto el software que se haya permitido explícitamente. Cualquier archivo que desea abrir tiene que tener la regla de directivas de una restricción de software que le permite abrir.

-   Para impedir que los administradores se bloqueen a sí mismos fuera del sistema, cuando el nivel de seguridad predeterminado se establece en **no permitido**, cuatro reglas de ruta de acceso del registro se crean automáticamente. Puede eliminar o modificar estas reglas de ruta de acceso del registro; Sin embargo, esto no se recomienda.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Para obtener una mayor seguridad, utilice las listas de control de acceso junto con las directivas de restricción de software.

-   Los usuarios podrían intentar eludir las directivas de restricción de software al cambiar el nombre o mover archivos no permitidos o sobrescriben archivos sin restricciones. Como resultado, se recomienda que use listas de control de acceso (ACL) para denegar a los usuarios el acceso necesario para realizar estas tareas.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Pruebe minuciosamente la nueva configuración de directiva en entornos de prueba antes de aplicar la configuración de directiva a su dominio.

-   Nueva configuración de directiva podría actuar diferente al esperado originalmente. Las pruebas reducen el riesgo de encontrar un problema al implementar la configuración de directiva en toda la red.

-   Puede configurar un dominio de prueba, independiente del dominio de su organización, en el que se va a probar la nueva configuración de directiva. También puede probar la configuración de directiva mediante la creación de un GPO de prueba y vincularlo a una unidad organizativa de prueba. Cuando haya probado exhaustivamente la configuración de directiva con usuarios de prueba, puede vincular el GPO de prueba a su dominio.

-   No establezca programas o archivos que **no permitido** sin realizar pruebas para ver cuál puede ser el efecto. Restricciones en determinados archivos pueden verse seriamente afectado el funcionamiento de su equipo o red.

-   La información que se han escrito incorrectamente o errores de escritura pueden dar lugar a una configuración de directiva no funciona según lo previsto. Probar la nueva configuración de directiva antes de aplicarlas puede evitar un comportamiento inesperado.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtrar según la pertenencia a grupos de seguridad de configuración de directiva de usuario.

-   Puede especificar usuarios o grupos para los que no desea una configuración de directiva para aplicar desactivando el **aplicar directiva de grupo** y **lectura** casillas de verificación que se encuentran en el **seguridad**ficha del cuadro de diálogo Propiedades para el GPO.

-   Cuando se deniega el permiso de lectura, la configuración de directiva no se descarga el equipo. Como resultado, se consume menos ancho de banda mediante la descarga de la configuración de directiva innecesarios, lo que permite que la red funcione más rápidamente. Para denegar el permiso de lectura, seleccione **Deny** para el **lectura** casilla de verificación que se encuentra en la **seguridad** ficha del cuadro de diálogo Propiedades para el GPO.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>No crear vínculo a un GPO en otro dominio o sitio.

-   Vincular a un GPO en otro dominio o sitio puede causar un bajo rendimiento.

## <a name="additional-resources"></a>Recursos adicionales

|Tipo de contenido|Referencias|
|--------|-------|
|**Planeamiento**|[Referencia técnica de directivas de restricción de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Operaciones**|[Administrar las directivas de restricción de Software](administer-software-restriction-policies.md)|
|**Solución de problemas**|[Directivas de restricción de software, solución de problemas (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Seguridad**|[Amenazas y contramedidas para la restricción de Software las directivas (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Amenazas y contramedidas para la restricción de Software las directivas (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Herramientas y configuración**|[Las directivas de restricción de software herramientas y configuración (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Recursos de la comunidad**|[Bloqueo de la aplicación con directivas de restricción de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


