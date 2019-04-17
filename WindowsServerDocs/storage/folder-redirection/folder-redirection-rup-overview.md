---
title: Introducción a la redirección de carpetas, archivos sin conexión y perfiles de usuario móvil
description: Una descripción general de las tecnologías de redirección de carpetas, archivos sin conexión y perfiles de usuario móvil.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4e3cf32cd718b906f16fc09901284d8520177df8
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233501"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Introducción a la redirección de carpetas, archivos sin conexión y perfiles de usuario móvil

>Se aplica a: 10 de Windows, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

En este tema se describe la redirección de carpetas, archivos sin conexión (CSC o almacenamiento en caché de cliente) y tecnologías de perfiles de usuario móvil (a veces conocidos como RUP), incluidos cuáles son las novedades y dónde encontrar información adicional.

## <a name="technology-description"></a>Descripción de la tecnología

El redireccionamiento de carpetas y los archivos sin conexión se usan conjuntamente para redireccionar la ruta de acceso de las carpetas locales (como la carpeta Mis documentos) a una ubicación de red, mientras se almacena en memoria caché local el contenido para lograr una mayor velocidad y disponibilidad. Los perfiles de usuarios móviles se usan para redireccionar un perfil de usuario a una ubicación de red. Estas características se usan para hacer referencia como Intellimirror.

- **Redirección de carpetas** permite a los usuarios y administradores redirigir la ruta de acceso de una carpeta conocida a una nueva ubicación, manualmente o mediante el uso de directiva de grupo. La nueva ubicación puede ser una carpeta en el equipo local o un directorio en un recurso compartido de archivos. Los usuarios interactúan con los archivos en la carpeta redirigida como si aún existía en la unidad local. Por ejemplo, puede redirigir la carpeta de documentos, que normalmente se almacena en una unidad local, en una ubicación de red. Los archivos en la carpeta, a continuación, están disponibles para el usuario desde cualquier equipo en la red.
- **Archivos sin conexión** hace que los archivos de red disponibles para un usuario, incluso si la conexión de red para el servidor es lenta o no está disponible. Cuando se trabaja en línea, rendimiento de acceso del archivo es a la velocidad de la red y el servidor. Al trabajar sin conexión, se recuperan los archivos de la carpeta de archivos sin conexión a velocidades de acceso local. Un equipo pasa al modo sin conexión cuando:
  - Se ha habilitado el modo **Siempre sin conexión**
  - El servidor no está disponible
  - La conexión de red es más lenta que un umbral configurable
  - El usuario cambia manualmente al modo sin conexión mediante el botón **trabajar sin conexión** en el Explorador de Windows
- **Los perfiles de usuario móviles** redirige los perfiles de usuario a un recurso compartido de archivos para que los usuarios reciban el mismo sistema operativo y la configuración de la aplicación en varios equipos. Cuando un usuario inicia sesión en un equipo con una cuenta que se configura con un recurso compartido de archivos como la ruta de acceso de perfil, el perfil del usuario es descargado en el equipo local y se combina con el perfil local (si está presente). Cuando el usuario cierra la sesión en el equipo, la copia local de su perfil, incluidos los cambios, se combina con la copia del servidor del perfil. Normalmente, un administrador de red habilita los perfiles de usuario móviles en cuentas de dominio.

## <a name="practical-applications"></a>Aplicaciones prácticas

Los administradores pueden usar la redirección de carpetas, los archivos sin conexión y perfiles de usuario móvil para Centralice el almacenamiento de datos de usuario y la configuración y para proporcionar a los usuarios la capacidad de tener acceso a sus datos mientras sin conexión o si se produce una interrupción de red o al servidor. Algunas aplicaciones específicas incluyen:

- Centralizar los datos desde los equipos cliente para tareas administrativas, como el uso de una herramienta de copia de seguridad basada en servidor para realizar una copia de seguridad de configuración y las carpetas de usuario.
- Permitir a los usuarios seguir teniendo acceso a los archivos de red, incluso si hay una interrupción de red o al servidor.
- Optimizar el uso de ancho de banda y mejorar la experiencia de los usuarios en las sucursales que tienen acceso a los archivos y carpetas que se hospedan en servidores corporativos que se encuentra fuera del sitio.
- Permitir que los usuarios móviles tener acceso a los archivos de red mientras se trabaja sin conexión o a través de redes lentas.

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

En la siguiente tabla se describe algunos de los principales cambios en la redirección de carpetas, archivos sin conexión y perfiles de usuarios móviles que están disponibles en esta versión.

|Característica/funcionalidad|¿Nueva o actualizada?|Descripción|
|---|---|---|
|Modo siempre sin conexión|Nuevo|Proporciona un acceso más rápido a los archivos y el uso de ancho de banda inferior siempre trabajando sin conexión, incluso cuando se conecta a través de una conexión de red de alta velocidad.|
|Tenga en cuenta el costo de sincronización|Nuevo|Ayuda a los usuarios a evitar los costos de uso de datos alta de la sincronización durante el uso de las conexiones intencionadas que tienen límites de uso, o durante la movilidad en la red del proveedor de otra.|
|Soporte técnico de equipo principal|Nuevo|Permite limitar el uso de la redirección de carpetas, perfiles de usuario móvil o ambos a sólo los equipos principal de un usuario.|

## <a name="always-offline-mode"></a>Modo siempre sin conexión

A partir de Windows 8 y Windows Server 2012, los administradores pueden configurar la experiencia para los usuarios de los archivos sin conexión siempre trabajar sin conexión, incluso cuando se conectan a través de una conexión de red de alta velocidad. Windows actualiza los archivos en la memoria caché de archivos sin conexión mediante la sincronización de cada hora en segundo plano, de forma predeterminada.

### <a name="what-value-does-always-offline-mode-add"></a>¿Qué valor es siempre sin conexión modo agregar?

El modo siempre sin conexión ofrece las siguientes ventajas:

- Los usuarios experimentan un acceso más rápido a los archivos de carpetas redirigidas, como la carpeta de documentos.
- Se reduce el ancho de banda de red, lo que reduce los costos en conexiones WAN costosas o conexiones intencionadas como una red móvil 4 G.

### <a name="how-has-always-offline-mode-changed-things"></a>¿Cómo ha cambiado siempre sin conexión modo cosas?

Antes de Windows 8, Windows Server 2012, los usuarios debería realizar la transición entre los modos en línea y sin conexión, según la disponibilidad de la red y condiciones, incluso cuando el modo de vínculo de baja velocidad (también conocido como el modo de conexión lenta) se ha habilitado y configurado con un 1 milisegundo umbral de latencia.

Con el modo siempre sin conexión, los equipos nunca realizar la transición al modo en línea cuando se configura la configuración de directiva de grupo **Configurar el modo de vínculo de baja velocidad** y se establece el parámetro de umbral de **latencia** a 1 milisegundo. Los cambios se sincronizan en segundo plano cada 120 minutos, de forma predeterminada, pero la sincronización es configurable mediante el uso de la configuración de directiva de grupo de **Configuración de sincronización de fondo** .

Para obtener más información, vea [Habilitar el siempre modo sin conexión para proporcionar más rápido acceso a los archivos](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Tenga en cuenta el costo de sincronización

Con la sincronización de tener en cuenta de costo, Windows deshabilita la sincronización en segundo plano cuando el usuario está utilizando una conexión de red intencionadas, como una red móvil de 4G y el suscriptor es cerca o a través de su límite de ancho de banda o móviles en la red del proveedor de otra.

>[!NOTE]
>Conexiones de red intencionadas suelen tengan las latencias de ida y vuelta de red que son más lentas que el valor de latencia de 35 milisegundos predeterminado para realizar una transición al modo sin conexión (conexión lenta) en Windows 8, Windows Server 2012 y Windows Server 2016. Por lo tanto, estas conexiones suelen hacer la transición al modo sin conexión (conexión lenta) automáticamente.

### <a name="what-value-does-cost-aware-synchronization-add"></a>¿Qué valor se agrega sincronización de costo?

Tenga en cuenta el costo de sincronización ayuda a los usuarios a evitar los costos de uso de datos inesperadamente alta durante el uso de las conexiones intencionadas que tienen límites de uso, o durante la movilidad en la red del proveedor de otra.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>¿Cómo ha cambiado cosas en cuenta el costo de sincronización?

Antes de Windows 8 y Windows Server 2012, los usuarios que deseaban minimizar las cuotas durante el uso de archivos sin conexión en conexiones de red intencionadas tuvieron que realizar un seguimiento de su uso de datos mediante las herramientas del proveedor de red móvil. Los usuarios, a continuación, podrían cambiar manualmente al modo sin conexión cuando se han de movilidad, cerca de su límite de ancho de banda o por encima de su límite.

Con la sincronización de tener en cuenta de costo, Windows automáticamente realiza un seguimiento de límites de uso de ancho de banda y movilidad mientras en conexiones intencionadas. Cuando el usuario se desplaza cerca de su límite de ancho de banda, o a través de su límite, Windows pasa a modo sin conexión y se evita que toda la sincronización. Los usuarios pueden iniciar manualmente la sincronización, y los administradores pueden reemplazar tenga en cuenta el costo de sincronización para usuarios específicos, como los ejecutivos.

Para obtener más información, vea [Habilitar la sincronización de archivos de fondo en las redes medido](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Equipos principales para la redirección de carpetas y perfiles de usuario móvil

Ahora puede designar un conjunto de equipos, conocidos como equipos principales, para cada usuario de dominio, que le permite controlar qué equipos utilizar redirección de carpetas, perfiles de usuario móvil o ambos. La designación de los equipos principales es un método simple y eficaz para asociar los datos de usuario y la configuración con determinados equipos o dispositivos, simplificar la visión general del administrador, mejorar la seguridad de los datos y ayudar a proteger los perfiles de usuario de daños.

### <a name="what-value-do-primary-computers-add"></a>¿Qué valor agregar equipos principales?

Hay cuatro ventajas principales a la designación de los equipos principales para los usuarios:

- El administrador puede especificar qué usuarios pueden utilizar para tener acceso a sus datos redirigido y la configuración de los equipos. Por ejemplo, el administrador puede elegir para variar los datos de usuario y la configuración entre un usuario de escritorio y portátiles y no traspasar entre la información cuando ese usuario inicia sesión en cualquier equipo, como un equipo de la sala de conferencia.
- La designación de los equipos principales reduce la seguridad y el riesgo de privacidad de dejar datos personales o corporativos residuales en equipos donde el usuario ha iniciado sesión. Por ejemplo, un director general que inicie sesión en el equipo de un empleado para acceso temporal no deje detrás de todos los datos personales o corporativos.
- Equipos principales habilitan el administrador para mitigar el riesgo de configurado incorrectamente o en caso contrario, perfil dañado, lo que podría provocar de movilidad de manera diferente entre sistemas configurados, como entre equipos x86o y basados en x64 bits.
- La cantidad de tiempo necesario para primer inicio de un usuario sesión en un equipo no es el principal, por ejemplo, un servidor, es más rápida debido a que el usuario perfil de usuario móvil o carpetas redirigidas no se descargan. También se reducen los tiempos de cierre de sesión, debido a que no es necesario que los cambios realizados en el perfil de usuario se cargan en el recurso compartido de archivos.

### <a name="how-have-primary-computers-changed-things"></a>¿Cómo han cambiado los equipos principales cosas?

Para limitar la descarga de datos privados del usuario en los equipos de principales, las tecnologías de redirección de carpetas y perfiles de usuario móviles realizan las siguientes comprobaciones de lógica cuando un usuario inicia sesión en un equipo:

1. El sistema operativo Windows comprueba la nueva configuración de directiva de grupo (**descarga los perfiles móviles principales sólo en equipos** y **redirigir las carpetas principales sólo en equipos**) para determinar si el atributo **MsDS-principal-equipo** activo Servicios de dominio de Active Directory (AD DS) debe influir en la decisión de se desplazan el perfil del usuario o aplicar la redirección de carpetas.
2. Si la configuración de directiva permite soporte técnico principal del equipo, Windows comprueba que el esquema de AD DS es compatible con el atributo **MsDS-principal-equipo** . Si es así, Windows determina si el equipo que el usuario está iniciando sesión en se designa como un equipo principal para el usuario de la siguiente manera:
    1. Si el equipo es uno de los equipos del usuario principal, Windows aplica la configuración de perfiles de usuario móviles y redirección de carpetas.
    2. Si el equipo no es uno de los equipos del usuario principal, Windows carga el perfil del usuario almacenado en caché local, si está presente, o crea un nuevo perfil local. Windows también quita cualquier carpetas redirigidas existentes según la acción de eliminación que se ha especificado por la configuración de directiva de grupo aplicada previamente, que se conserva en la configuración de redirección de carpetas local.

Para obtener más información, vea [Implementar equipos principal para la redirección de carpetas y perfiles de usuario móvil](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Requisitos de hardware

Redirección de carpetas, archivos sin conexión y los perfiles de usuario móviles requieren un equipo basados en x64 bits o x86o, y no son compatibles con Windows en equipos basados en WOA de ARM.

## <a name="software-requirements"></a>Requisitos de software

Para designar los equipos principales, el entorno debe cumplir los siguientes requisitos:

- Se debe actualizar el esquema de los servicios de dominio de Active Directory (AD DS) para incluir el esquema de Windows Server 2012 y condiciones (instalar un Windows Server 2012 o posterior controlador de dominio actualizará automáticamente el esquema). Para obtener más información acerca de cómo actualizar el esquema de AD DS, vea [Actualizar los controladores de dominio a Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, 2016 de Windows Server, Windows Server 2012 R2 o Windows Server 2012 y se unen al dominio de Active Directory que se está administrando.

## <a name="more-information"></a>Más información

Para obtener más información relacionada, consulta los siguientes recursos.

|Tipo de contenido|Referencias|
|---|---|
|Evaluación del producto|[Compatibilidad con trabajadores de la información con servicios de archivos confiable y almacenamiento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[What ' s New in archivos sin conexión](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 y Windows Server 2008 R2)<br>[Novedades en los archivos sin conexión para Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Cambios realizados en los archivos sin conexión en Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine)|
|Implementación|[Implementación de redirección de carpetas, archivos sin conexión y perfiles de usuario móvil](deploy-folder-redirection.md)<br>[Implementación de una solución de centralización de datos para el usuario final: redirección de carpetas y validación de la tecnología de archivos sin conexión e implementación](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Administración de movilidad de la Guía de implementación de datos de usuario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Configurar nuevos sin conexión archivos de características para la Guía paso a paso de los equipos de Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Uso de redirección de carpetas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implementación de redirección de carpetas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003)|
|Herramientas y configuración|[Archivos sin conexión en MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Referencia de la directiva de grupo de archivos sin conexión](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000)|
|Recursos de la comunidad|[Foro de almacenamiento y servicios de archivos](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[¡Hola, Scripting Guy! ¿Cómo puedo trabajar con la característica de archivos sin conexión en Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[¡Hola, Scripting Guy! ¿Cómo se puede habilitar y deshabilitar los archivos sin conexión?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)|
Tecnologías relacionadas|[Identidad y acceso en Windows Server](../../identity/identity-and-access.md)<br>[Almacenamiento en Windows Server](../storage.md)<br>[Administración de acceso y servidor remotos](../../remote/index.md)|