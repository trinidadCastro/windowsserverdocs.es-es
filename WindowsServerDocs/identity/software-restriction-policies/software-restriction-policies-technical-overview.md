---
title: Información técnica de Directivas de restricción de software
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f98075cd8e662b3344f426bd8d69181994096a5f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953054"
---
# <a name="software-restriction-policies-technical-overview"></a>Información técnica de Directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describen las directivas de restricción de software, Cuándo y cómo usar la característica, qué cambios se han implementado en las versiones anteriores y se proporcionan vínculos a recursos adicionales que le ayudarán a crear e implementar directivas de restricción de software a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software proporcionan a los administradores un mecanismo controlado por directiva de grupo para identificar el software y controlar su capacidad de ejecución en el equipo local. Estas directivas se pueden usar para proteger los equipos que ejecutan sistemas operativos Microsoft Windows (a partir de Windows Server 2003 y Windows XP Professional) frente a conflictos conocidos y proteger los equipos frente a amenazas de seguridad como virus malintencionados y programas troyanos. También puede usar las directivas de restricción de software para crear una configuración altamente restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Las directivas de restricción de software están integradas en Microsoft Active Directory y la directiva de grupo. También puede crear directivas de restricción de software en equipos independientes.

Las directivas de restricción de software son directivas de confianza, las cuáles son normativas establecidas por un administrador para restringir los scripts y otros códigos que no son de confianza plena. La extensión de directivas de restricción de software para el Editor de directivas de grupo local proporciona una única interfaz de usuario a través de la cual la configuración para restringir el uso de aplicaciones puede administrarse en el equipo local o en todo un dominio.

## <a name="procedures"></a>Procedimientos

-   [Administración de las directivas de restricción de software](administer-software-restriction-policies.md)

    -   [Determinar la lista de permitidos y el inventario de aplicaciones para las directivas de restricción de software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Trabajo con reglas de directivas de restricción de software](work-with-software-restriction-policies-rules.md)

    -   [Usar directivas de restricción de software para ayudar a proteger el equipo contra un virus de correo electrónico](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Solución de problemas de las directivas de restricción de software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Escenarios de uso de directivas de restricción de software
Los usuarios empresariales colaboran mediante correo electrónico, mensajería instantánea y aplicaciones punto a punto. A medida que estas colaboraciones aumentan, especialmente con el uso de Internet en la informática empresarial, se trata de amenazas de código malintencionado, como gusanos, virus y amenazas de atacantes o usuarios malintencionados.

Los usuarios pueden recibir código hostil en muchas formas, desde archivos ejecutables nativos de Windows (archivos. exe) hasta macros de documentos (como archivos. doc), hasta scripts (como archivos. vbs). Los usuarios malintencionados o los atacantes suelen usar métodos de ingeniería social para que los usuarios puedan ejecutar código que contiene virus y gusanos. (Ingeniería social es un término para engañar a los usuarios para que revelen su contraseña o algún tipo de información de seguridad). Si este código está activado, puede generar ataques por denegación de servicio en la red, enviar datos confidenciales o privados a Internet, poner en riesgo la seguridad del equipo o dañar el contenido de la unidad de disco duro.

Los usuarios y las organizaciones de TI deben ser capaces de determinar qué software se puede ejecutar y cuáles no. Con los grandes números y formas que puede llevar a cabo el código hostil, esto se convierte en una tarea difícil.

Las organizaciones pueden implementar directivas de restricción de software como parte de la estrategia de seguridad general para ayudar a proteger los equipos de la red de código hostil y software desconocido o no admitido.

Los administradores pueden usar las directivas de restricción de software para realizar las siguientes tareas:

-   Definir el código de confianza

-   Diseñar una directiva de grupo flexible para regular scripts, archivos ejecutables y controles ActiveX

El sistema operativo y las aplicaciones (como las aplicaciones de scripting) que cumplen con las directivas de restricción de software aplican las directivas de restricción de software.

Concretamente, los administradores pueden usar las directivas de restricción de software con los siguientes propósitos:

-   Especificar qué software (archivos ejecutables) se puede ejecutar en los equipos cliente

-   Evitar que los usuarios ejecuten programas específicos en equipos compartidos.

-   Especificar quién puede Agregar publicadores de confianza a los equipos cliente

-   Establecer el ámbito de las directivas de restricción de software (especifique si las directivas afectan a todos los usuarios o a un subconjunto de usuarios en los equipos cliente)

-   Evitar que los archivos ejecutables se ejecuten en el equipo local, la unidad organizativa (OU), el sitio o el dominio. Esto resultaría apropiado cuando no está usando directivas de restricción de software para tratar problemas potenciales con usuarios malintencionados.

## <a name="differences-and-changes-in-functionality"></a><a name="BKMK_Diffs_Changes"></a>Diferencias y cambios en la funcionalidad
No hay ningún cambio en la funcionalidad de SRP para Windows Server 2012 y Windows 8.

**Versiones admitidas**

Las directivas de restricción de software solo se pueden configurar y aplicar en equipos que ejecuten como mínimo Windows Server 2003, incluido Windows Server 2012 y, como mínimo, Windows XP, incluido Windows 8.

> [!NOTE]
> Ciertas ediciones del sistema operativo cliente de Windows a partir de Windows Vista no tienen directivas de restricciones de software. Es posible que los equipos no administrados en un dominio por directiva de grupo no reciban directivas distribuidas.

**Comparación de las funciones de control de aplicaciones en Directivas de restricción de software y AppLocker**

En la tabla siguiente se comparan las características y funciones de la característica Directivas de restricción de software (SRP) y AppLocker.

|Función de control de aplicaciones|SRP|AppLocker|
|----------------|----|-------|
|Ámbito|Las directivas de SRP se pueden aplicar a todos los sistemas operativos Windows a partir de Windows XP y Windows Server 2003.|Las directivas de AppLocker solo se aplican a Windows Server 2008 R2, Windows Server 2012, Windows 7 y Windows 8.|
|Creación de directivas|Las directivas de SRP se mantienen a través de la directiva de grupo y solo el administrador del GPO puede actualizar la directiva de SRP. El administrador del equipo local puede modificar las directivas de SRP definidas en el GPO local.|Las directivas de AppLocker se mantienen a través de la directiva de grupo y solo el administrador del GPO puede actualizar la directiva. El administrador del equipo local puede modificar las directivas de AppLocker definidas en el GPO local.<p>AppLocker permite la personalización de mensajes de error con el fin de dirigir a los usuarios a una página web para obtener ayuda.|
|Mantenimiento de directivas|Las directivas de SRP deben actualizarse mediante el complemento de directiva de seguridad local (si se crean las directivas localmente) o la consola de administración de directiva de grupo (GPMC).|Las directivas de AppLocker pueden actualizarse mediante el complemento de directiva de seguridad local (si se crean las directivas localmente), los GPMC o los cmdlets de Windows PowerShell AppLocker.|
|Aplicación de la directiva|Las directivas de SRP se distribuyen a través de la directiva de grupo.|Las directivas de AppLocker se distribuyen a través de la directiva de grupo.|
|Modo de cumplimiento|SRP funciona en el modo de lista de denegación, donde los administradores pueden crear reglas para los archivos que no desean permitir en esta empresa, mientras que el resto del archivo se puede ejecutar de forma predeterminada.<p>SRP también puede configurarse en "permitir modo de lista" de forma que todos los archivos estén bloqueados de forma predeterminada y los administradores tengan que crear reglas de permiso para los archivos que desea permitir.|De forma predeterminada, AppLocker funciona en el modo de lista de permitidos, donde solo se permite ejecutar los archivos para los que hay una regla de permiso coincidente.|
|Tipos de archivo que se pueden controlar|SRP puede controlar los siguientes tipos de archivo:<p>-Ejecutables<br />-Dll<br />-Scripts<br />: Instaladores de Windows<p>SRP no puede controlar cada tipo de archivo por separado. Todas las reglas de SRP están en una colección de regla única.|AppLocker puede controlar los siguientes tipos de archivo:<p>-Ejecutables<br />-Dll<br />-Scripts<br />: Instaladores de Windows<br />-Aplicaciones e instaladores empaquetados (Windows Server 2012 y Windows 8)<p>AppLocker mantiene una colección de reglas independientes para cada uno de los cinco tipos de archivo.|
|Tipos de archivo designados|SRP admite una lista extensible de tipos de archivo que se consideran ejecutables. Los administradores pueden agregar extensiones a los archivos que deben considerarse ejecutables.|AppLocker no admite esto. AppLocker admite actualmente las siguientes extensiones de archivo:<p>-Ejecutables (. exe,. com)<br />-DLL (. ocx,. dll)<br />-Scripts (. vbs,. js,. ps1,. cmd,. bat)<br />-Instaladores de Windows (. msi,. MST,. msp)<br />-Instaladores de aplicaciones empaquetadas (. appx)|
|Tipos de regla|SRP admite cuatro tipos de reglas:<p>-Hash<br />-Path<br />-Signature<br />-Zona de Internet|AppLocker admite tres tipos de reglas:<p>-Hash<br />-Path<br />-Publicador|
|Editar el valor de hash|SRP permite a los administradores proporcionar valores hash personalizados.|AppLocker calcula el valor de hash por su cuenta. Utiliza internamente el hash SHA1 Authenticode para los archivos ejecutables portables (exe y dll) y los instaladores de Windows y un hash de archivo sin formato SHA1 para el resto.|
|Compatibilidad con diferentes niveles de seguridad|Los administradores de SRP pueden especificar los permisos con los que se puede ejecutar una aplicación. Por lo tanto, un administrador puede configurar una regla de modo que el Bloc de notas siempre se ejecute con permisos restringidos y nunca con privilegios de administrador.<p>SRP en Windows Vista y varios niveles de seguridad anteriores compatibles. En Windows 7, esa lista se restringe a solo dos niveles: no permitido y no restringido (el usuario básico se traduce en no permitido).|AppLocker no admite niveles de seguridad.|
|Administración de aplicaciones empaquetadas e instaladores de aplicaciones empaquetadas|No se puede|.appx es un tipo de archivo válido que puede administrar AppLocker.|
|Selección del destino de una regla para un usuario o un grupo de usuarios|Las reglas de SRP se aplican a todos los usuarios en un equipo determinado.|Las reglas de AppLocker pueden destinarse a un usuario específico o un grupo de usuarios.|
|Compatibilidad con las excepciones de regla|SRP no admite excepciones de reglas.|Las reglas de AppLocker pueden tener excepciones que permiten a los administradores crear reglas como "permitir todo desde Windows excepto para Regedit.exe".|
|Compatibilidad con el modo auditoría|SRP no admite el modo auditoría. La única manera de probar las directivas de SRP es configurar un entorno de prueba y ejecutar algunos experimentos.|AppLocker admite el modo de auditoría, que permite a los administradores probar el efecto de la directiva en el entorno de producción real sin afectar a la experiencia del usuario. Una vez que estés satisfecho con los resultados, puedes iniciar la aplicación de la directiva.|
|Compatibilidad para exportar e importar directivas|SRP no admite la importación o la exportación de directivas.|AppLocker admite la importación y la exportación de directivas. Esto te permite crear la directiva de AppLocker en un equipo de muestra, probarla, exportar esa directiva y volver a importarla al GPO deseado.|
|Aplicación de reglas|De forma interna, el cumplimiento de las reglas de SRP se realiza en el modo de usuario, que es menos seguro.|Internamente, las reglas de AppLocker para archivos exe y dll se aplican en el modo kernel, que es más seguro que aplicarlas en el modo de usuario.|

## <a name="system-requirements"></a>Requisitos del sistema
Las directivas de restricción de software solo se pueden configurar y aplicar en equipos que ejecuten como mínimo Windows Server 2003 y Windows XP como mínimo. Directiva de grupo es necesario para distribuir directiva de grupo objetos que contienen directivas de restricción de software.

## <a name="software-restriction-policies-components-and-architecture"></a>Arquitectura y componentes de directivas de restricción de software
Las directivas de restricción de software proporcionan un mecanismo para que el sistema operativo y las aplicaciones compatibles con las directivas de restricción de software restrinjan la ejecución en tiempo de ejecución de los programas de software.

En un nivel alto, las directivas de restricción de software constan de los siguientes componentes:

-   API de directivas de restricción de software. Las interfaces de programación de aplicaciones (API) se usan para crear y configurar las reglas que constituyen la Directiva de restricción de software. También hay API de directivas de restricción de software para la consulta, el procesamiento y la aplicación de directivas de restricción de software.

-   Una herramienta de administración de directivas de restricción de software. Se compone de la extensión de **directivas de restricción de software** del complemento de **Editor de objetos de directiva de grupo local** , que los administradores utilizan para crear y editar las directivas de restricción de software.

-   Un conjunto de aplicaciones y API del sistema operativo que llaman a las API de directivas de restricción de software para proporcionar la aplicación de las directivas de restricción de software en tiempo de ejecución.

-   Active Directory y directiva de grupo. Las directivas de restricción de software dependen de la infraestructura de directiva de grupo para propagar las directivas de restricción de software del Active Directory a los clientes adecuados y para el ámbito y el filtrado de la aplicación de estas directivas en los equipos de destino adecuados.

-   Authenticode y WinVerify confían en las API que se usan para procesar archivos ejecutables firmados.

-   Visor de eventos. Las funciones que usan las directivas de restricción de software registran los eventos en los registros de Visor de eventos.

-   Conjunto resultante de directivas (RSoP), que puede ayudar en el diagnóstico de la Directiva vigente que se aplicará a un cliente.

Para obtener más información acerca de la arquitectura de SRP, cómo administra SRP las reglas, los procesos y las interacciones, vea [Cómo funcionan las directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2003/cc786941(v=ws.10)) en la biblioteca técnica de Windows Server 2003.

## <a name="best-practices"></a><a name="BKMK_Best_Practices"></a>Procedimientos recomendados

### <a name="do-not-modify-the-default-domain-policy"></a>No modifique la directiva predeterminada de dominio.

-   Si no edita la Directiva de dominio predeterminada, siempre tiene la opción de volver a aplicar la Directiva de dominio predeterminada si algo va mal con la Directiva de dominio personalizada.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Cree un objeto de directiva de grupo independiente para las directivas de restricción de software.

-   Si crea un objeto de directiva de grupo (GPO) independiente para las directivas de restricción de software, puede deshabilitar las directivas de restricción de software en caso de emergencia sin deshabilitar el resto de la Directiva de dominio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Si tiene problemas con la configuración de la Directiva aplicada, reinicie Windows en modo seguro.

-   Las directivas de restricción de software no se aplican cuando Windows se inicia en modo seguro. Si bloquea accidentalmente una estación de trabajo con directivas de restricción de software, reinicie el equipo en modo seguro, inicie sesión como administrador local, modifique la Directiva, ejecute **gpupdate**, reinicie el equipo y, a continuación, inicie sesión con normalidad.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Tenga cuidado al definir un valor predeterminado de no permitido.

-   Al definir un valor predeterminado de no **permitido**, no se permite todo el software excepto el software que se ha permitido explícitamente. Cualquier archivo que desee abrir debe tener una regla de directivas de restricción de software que permita que se abra.

-   Para evitar que los administradores se bloqueen fuera del sistema, cuando el nivel de seguridad predeterminado se establece en no **permitido**, se crean automáticamente cuatro reglas de ruta de acceso del registro. Puede eliminar o modificar estas reglas de ruta de acceso del registro; sin embargo, esto no es recomendable.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Para mayor seguridad, use listas de control de acceso junto con las directivas de restricción de software.

-   Es posible que los usuarios intenten eludir las directivas de restricción de software cambiando el nombre o moviendo los archivos no permitidos o sobrescribiendo archivos sin restricciones. Como resultado, se recomienda utilizar listas de control de acceso (ACL) para denegar a los usuarios el acceso necesario para realizar estas tareas.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Pruebe minuciosamente las nuevas configuraciones de directiva en entornos de prueba antes de aplicar la configuración de la Directiva a su dominio.

-   La nueva configuración de directiva podría actuar de manera diferente a la esperada originalmente. La prueba disminuye la posibilidad de encontrar un problema al implementar la configuración de directiva en la red.

-   Puede configurar un dominio de prueba, independiente del dominio de su organización, en el que probar la nueva configuración de directiva. También puede probar la configuración de la Directiva mediante la creación de un GPO de prueba y su vinculación a una unidad organizativa de prueba. Cuando haya probado exhaustivamente la configuración de directivas con usuarios de prueba, puede vincular el GPO de prueba a su dominio.

-   No establezca programas o archivos en no **permitidos** sin pruebas para ver cuál puede ser el efecto. Las restricciones en ciertos archivos pueden afectar seriamente al funcionamiento del equipo o de la red.

-   La información que se escribe incorrectamente o los errores tipográficos pueden dar lugar a una configuración de directiva que no funciona de la manera esperada. Probar la nueva configuración de Directiva antes de aplicarla puede impedir un comportamiento inesperado.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtre la configuración de directiva de usuario en función de la pertenencia a grupos de seguridad.

-   Puede especificar los usuarios o grupos a los que no desea aplicar una configuración de Directiva si desactiva las casillas **aplicar Directiva de grupo** y **leer** , que se encuentran en la pestaña **seguridad** del cuadro de diálogo Propiedades del GPO.

-   Cuando se deniega el permiso de lectura, el equipo no descarga la configuración de directiva. Como resultado, se consume menos ancho de banda descargando configuraciones de directivas innecesarias, lo que permite que la red funcione más rápidamente. Para denegar el permiso de lectura, seleccione **denegar** en la casilla **leer** , que se encuentra en la ficha **seguridad** del cuadro de diálogo Propiedades del GPO.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>No se vincula a un GPO en otro dominio o sitio.

-   La vinculación a un GPO en otro dominio o sitio puede dar lugar a un bajo rendimiento.

## <a name="additional-resources"></a>Recursos adicionales

|Tipo de contenido|Referencias|
|--------|-------|
|**Planeamiento**|[Referencia técnica de directivas de restricción de software](/previous-versions/windows/it-pro/windows-server-2003/cc728085(v=ws.10))|
|**Operaciones**|[Administración de las directivas de restricción de software](administer-software-restriction-policies.md)|
|**Solución de problemas**|[Solución de problemas de directivas de restricción de software (2003)](/previous-versions/windows/it-pro/windows-server-2003/cc737011(v=ws.10))|
|**Seguridad**|[Amenazas y contramedidas para las directivas de restricción de software (2008)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349795(v=ws.10))<p>[Amenazas y contramedidas para las directivas de restricción de software (2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125926(v=ws.10))|
|**Herramientas y configuración**|[Herramientas y configuración de las directivas de restricción de software (2003)](/previous-versions/windows/it-pro/windows-server-2003/cc782454(v=ws.10))|
|**Recursos de la comunidad**|[Bloqueo de la aplicación con directivas de restricción de software](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
