---
title: Introducción al redireccionamiento de carpetas, archivos sin conexión y perfiles de usuario móvil
description: Información general de las tecnologías de redirección de carpetas, Archivos sin conexión y perfiles de usuario móviles.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ae1b23244f141cd0806ee14d3c40117ba72aeebb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402061"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Introducción al redireccionamiento de carpetas, archivos sin conexión y perfiles de usuario móvil

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

En este tema se describen las tecnologías de redirección de carpetas, Archivos sin conexión (almacenamiento en caché del lado cliente o CSC) y perfiles de usuario móviles (a veces conocidos como RUP), incluidas las novedades y dónde encontrar información adicional.

## <a name="technology-description"></a>Descripción de la tecnología

El redireccionamiento de carpetas y los archivos sin conexión se usan conjuntamente para redireccionar la ruta de acceso de las carpetas locales (como la carpeta Mis documentos) a una ubicación de red, mientras se almacena en memoria caché local el contenido para lograr una mayor velocidad y disponibilidad. Los perfiles de usuarios móviles se usan para redireccionar un perfil de usuario a una ubicación de red. Estas características se conocían como Intellimirror.

- **La redirección** de carpetas permite a los usuarios y administradores redirigir la ruta de acceso de una carpeta conocida a una nueva ubicación, manualmente o mediante Directiva de grupo. La nueva ubicación puede ser una carpeta en el equipo local o un directorio en un recurso compartido de archivos. Los usuarios interactúan con los archivos en la carpeta redireccionada como si aún existiesen en la unidad local. Por ejemplo, es posible redirigir la carpeta Mis documentos, que normalmente está almacenada en la unidad de disco duro local del equipo, a una ubicación de red. De esta manera, los archivos en la carpeta están disponibles para el usuario desde cualquier equipo de la red.
- **Archivos sin conexión hace que**los archivos de red estén disponibles para un usuario, incluso si la conexión de red con el servidor no está disponible o es lenta.  Cuando se trabaja en línea, el rendimiento de acceso a archivos es el de la velocidad de la red y del servidor. Cuando se trabaja sin conexión, los archivos se recuperan desde la carpeta Archivos sin conexión a velocidades de acceso local. El equipo cambia a modo Sin conexión cuando:
  - Se ha habilitado el modo **siempre sin conexión**
  - El servidor no está disponible
  - La conexión de red es más lenta que un umbral configurable
  - El usuario cambia al modo Sin conexión en forma manual mediante el botón **Trabajar sin conexión** en Windows Explorer
- **Los perfiles** de usuario móviles redirigen los perfiles de usuario a un recurso compartido de archivos para que los usuarios reciban la misma configuración del sistema operativo y de la aplicación en varios equipos. Cuando un usuario inicia sesión en un equipo con una cuenta que está configurada con un recurso compartido de archivos como la ruta de acceso del perfil, el perfil del usuario se descarga en el equipo local y se combina con el perfil local (si está presente). Cuando el usuario cierra sesión en el equipo, se combina la copia local de su perfil, incluidos todos los cambios, con la copia del perfil en el servidor. Normalmente, un administrador de red habilita los perfiles de usuario móviles en cuentas de dominio.

## <a name="practical-applications"></a>Aplicaciones prácticas

Los administradores pueden usar el redireccionamiento de carpetas, los archivos sin conexión y los perfiles de usuarios móviles para centralizar el almacenamiento de los datos y la configuración de los usuarios y proporcionales a estos la capacidad de tener acceso a sus archivos mientras se encuentran sin conexión o en el caso de una interrupción en la red o el servidor. Ejemplos de aplicaciones específicas:

- Centralización de datos de equipos de clientes para tareas administrativas, como el uso de una herramienta de copias de seguridad basada en el servidor para hacer una copia de seguridad de las carpetas y la configuración del usuario.
- Permitir que los usuarios continúen teniendo acceso a los archivos de la red, aún en el caso de una interrupción en la red o el servidor.
- Optimizar el uso del ancho banda y mejorar la experiencia de los usuarios en sucursales que tienen acceso a los archivos y carpetas hospedados en servidores corporativos de otras ubicaciones.
- Permitir que los usuarios móviles tengan acceso a los archivos de red mientras trabajan sin conexión o a través de redes lentas.

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

En la siguiente tabla, se describen algunos de los principales cambios en el redireccionamiento de carpetas, archivos sin conexión y perfiles de usuarios móviles que se encuentran disponibles en esta versión.

| Característica/funcionalidad | ¿Nueva o actualizada? | Descripción |
| --- | --- | --- |
| Modo Siempre sin conexión | Nuevo | Proporciona un acceso más rápido a los archivos y un menor uso de ancho de banda ya que siempre trabaja sin conexión, incluso cuando se encuentra conectado a través de una conexión de red de alta velocidad. |
| Sincronización con control costo | Nuevo | Ayuda a los usuarios a evitar los costos de uso de datos elevados de la sincronización al usar conexiones de uso medido que tienen límites de uso, o en itinerancia en la red de otro proveedor. |
| Soporte de equipo principal | Nuevo | Permite limitar el uso de la redirección de carpetas, los perfiles de usuario móviles o ambos a los equipos principales de un usuario. |

## <a name="always-offline-mode"></a>Modo Siempre sin conexión

A partir de Windows 8 y Windows Server 2012, los administradores pueden configurar la experiencia de los usuarios de Archivos sin conexión para que siempre trabajen sin conexión, incluso cuando estén conectados a través de una conexión de red de alta velocidad. De manera predeterminada, Windows actualiza los archivos en la memoria caché de los archivos sin conexión, mediante la sincronización, cada hora en segundo plano.

### <a name="what-value-does-always-offline-mode-add"></a>¿Qué valor tiene el modo sin conexión siempre agregado?

El modo Siempre sin conexión proporciona los siguientes beneficios:

- Los usuarios experimentan un acceso más rápido a los archivos en carpetas redireccionadas, como por ejemplo la carpeta Mis documentos.
- Se reduce el ancho de banda de la red, lo que disminuye los costos de conexiones WAN caras o conexiones de uso medido como la red móvil 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>¿Cómo ha cambiado siempre el modo sin conexión?

Antes de Windows 8, Windows Server 2012, los usuarios pasaban de los modos en línea y sin conexión, según la disponibilidad y las condiciones de la red, aunque el modo de vínculo de baja velocidad (también conocido como modo de conexión lenta) estuviera habilitado y establecido en 1 milisegundo. umbral de latencia.

Con el modo siempre sin conexión, los equipos nunca pasan al modo en línea cuando se configura la opción **configurar el modo de vínculo de baja velocidad** Directiva de grupo y el parámetro umbral de **latencia** se establece en 1 milisegundo. Los cambios se sincronizan en segundo plano cada 120 minutos, de manera predeterminada, pero la sincronización puede definirse mediante la configuración de directiva de grupo **Configurar sincronización en segundo plano**.

Para obtener más información, consulte el tema sobre [habilitar el modo Siempre sin conexión para proporcionar un acceso más rápido a los archivos](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Sincronización con control costo

Con la sincronización con reconocimiento de costo, Windows deshabilita la sincronización en segundo plano cuando el usuario utiliza una conexión de red de uso medido, como una red móvil 4G, y el suscriptor está cerca o superado su límite de ancho de banda o se mueve en la red de otro proveedor.

> [!NOTE]
> Las conexiones de red de uso medido normalmente tienen latencias de red de ida y vuelta que son más lentas que el valor de latencia de 35 milisegundos predeterminado para la transición al modo sin conexión (conexión lenta) en Windows 8, Windows Server 2019, Windows Server 2016 y Windows Server 2012. Por lo tanto, estas conexiones generalmente hacen la transición al modo Sin conexión (Conexión lenta) en forma automática.

### <a name="what-value-does-cost-aware-synchronization-add"></a>¿Qué valor agrega la sincronización con reconocimiento de costos?

La sincronización con reconocimiento de costos ayuda a los usuarios a evitar costos inesperados de uso de datos mientras se usan conexiones de uso medido que tienen límites de uso, o bien se mueven en la red de otro proveedor.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>¿Cómo ha cambiado la sincronización con reconocimiento de costos?

Antes de Windows 8 y Windows Server 2012, los usuarios que deseaban minimizar las tarifas al usar Archivos sin conexión en las conexiones de red de uso medido debían realizar un seguimiento del uso de sus datos mediante herramientas del proveedor de red móvil. Los usuarios entonces podían cambiar manualmente al modo Sin conexión cuando trabajaban en forma móvil, cerca de su límite de ancho de banda o sobre este.

Con la sincronización con reconocimiento de costos, Windows realiza un seguimiento automático de los límites de uso de ancho de banda y roaming en las conexiones de uso medido. Cuando el usuario trabaja en forma móvil, cerca del límite de ancho de banda, o sobre este, Windows cambia al modo Sin conexión y evita todo tipo de sincronización. Los usuarios aún pueden iniciar manualmente la sincronización, y los administradores pueden anular la sincronización con control de costo para usuarios específicos, como los ejecutivos.

Para obtener más información, consulte [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Equipos principales para el redireccionamiento de carpetas y los perfiles de usuarios móviles

Ahora puede designar un conjunto de equipos, conocidos como equipos principales, para cada usuario de dominio, lo que le permite controlar qué equipos usan el redireccionamiento de carpetas, los perfiles de usuario móviles o ambos. La designación de equipos principales es un método simple y efectivo para asociar los datos y configuración de usuario con equipos o dispositivos particulares, simplificar la supervisión de los administradores, mejorar la seguridad de los datos y ayudar a proteger de daños los perfiles de usuario.

### <a name="what-value-do-primary-computers-add"></a>¿Qué valor agregan los equipos principales?

Hay cuatro beneficios importantes en la designación de equipos principales para los usuarios:

- El administrador puede especificar qué equipos pueden usar los usuarios para tener acceso a los datos y configuración redireccionados. Por ejemplo, el administrador puede elegir entre los datos de usuario y la configuración de itinerancia entre el escritorio y el equipo portátil de un usuario, y no desplazar la información cuando el usuario inicia sesión en otro equipo, como un equipo de sala de conferencias.
- La designación de equipos principales reduce el riesgo de seguridad y privacidad al dejar datos personales o corporativos residuales en equipos en los que el usuario ha iniciado sesión. Por ejemplo, un director general que inicia sesión en el equipo de un empleado para el acceso temporal no deja atrás ningún dato personal o corporativo.
- Los equipos principales permiten que el administrador mitigue el riesgo de que se presente un perfil configurado incorrectamente o dañado, lo que podría originarse al utilizar un perfil móvil entre sistemas configurados en forma diferente, como entre equipos basados en x86 o x64.
- La cantidad de tiempo necesario para el primer inicio de sesión de un usuario en un equipo no principal, como un servidor, es más rápida porque el perfil de usuario móvil del usuario y/o las carpetas redirigidas no se descargan. Los tiempos de cierre de sesión también se reducen, debido a que no es necesario cargar los cambios del perfil de usuario en el recurso compartido de archivos.

### <a name="how-have-primary-computers-changed-things"></a>¿Cómo cambian las cosas los equipos principales?

Para limitar la descarga de datos de usuario privados en equipos principales, las tecnologías de redireccionamiento de carpetas y perfiles de usuarios móviles realizan las siguientes comprobaciones lógicas cuando un usuario inicia sesión en un equipo:

1. El sistema operativo Windows comprueba la nueva configuración de directiva de grupo (**Descargar perfiles móviles solo en equipos principales** y **redirigir carpetas solo en equipos principales**) para determinar si el atributo **MsDS-Primary-Computer** está activo. Los servicios de dominio de directorio (AD DS) deben influir en la decisión de desplazar el perfil del usuario o aplicar la redirección de carpetas.
2. Si la configuración de directiva permite el soporte de equipo principal, Windows comprueba que el esquema AD DS admita el atributo **msDS-Primary-Computer**. Si lo admite, Windows determina de la siguiente manera si el equipo en el que está iniciando sesión el usuario se designa como equipo principal del usuario:
    1. Si el equipo es uno de los equipos principales del usuario, Windows aplica la configuración de perfiles de usuario móviles y redirección de carpetas.
    2. Si el equipo no es uno de los equipos principales del usuario, Windows carga el perfil local almacenado en caché del usuario, si está presente, o crea un nuevo perfil local. Windows también quita las carpetas redireccionadas existentes de acuerdo con la acción de eliminación que especificó la configuración de directiva de grupo aplicada previamente, que se mantiene en la configuración de redireccionamiento de carpeta local.

Para obtener más información, consulte [implementar equipos principales para redirección de carpetas y perfiles de usuario móviles](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Requisitos de hardware

El redireccionamiento de carpeta, los archivos sin conexión y los perfiles de usuarios móviles requieren un equipo basado en x64 o en x86, y no son compatibles con Windows en equipos basados en ARM (WOA).

## <a name="software-requirements"></a>Requisitos de software

Para designar equipos principales, el entorno debe cumplir con los siguientes requisitos:

- El esquema de Active Directory Domain Services (AD DS) debe actualizarse para incluir el esquema y las condiciones de Windows Server 2012 (la instalación de un controlador de dominio de Windows Server 2012 o posterior actualiza automáticamente el esquema). Para obtener más información acerca de cómo actualizar el esquema de AD DS, vea [actualizar controladores de dominio a Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y unirse al dominio de Active Directory que está administrando.

## <a name="more-information"></a>Más información

Para obtener más información relacionada, vea los siguientes recursos.

| Tipo de contenido | Referencias |
| --- | --- |
| Evaluación del producto | [Compatibilidad con los trabajadores de la información con almacenamiento y servicios de archivos confiables](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Novedades de archivos sin conexión](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 y Windows Server 2008 R2)<br>[Novedades de Archivos sin conexión para Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Cambios en archivos sin conexión en Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (Revista de TechNet) |
| Implementación | [Implementar el redireccionamiento de carpetas, los Archivos sin conexión y los perfiles de usuario móviles](deploy-folder-redirection.md)<br>[Implementar una solución de centralización de datos de usuario final: Validación e implementación de la tecnología de redirección de carpetas y Archivos sin conexión](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Guía de implementación de datos de usuarios móviles](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guía paso a paso para configurar nuevas características de Archivos sin conexión para equipos con Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Usar redirección de carpetas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implementación de la redirección de carpetas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Herramientas y configuración | [Archivos sin conexión en MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Referencia de directiva de grupo de archivos sin conexión](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Recursos de la comunidad | [Foro de almacenamiento y servicios de archivos](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[¡ Hola, chicos del scripting! ¿Cómo puedo trabajar con la característica Archivos sin conexión de Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[¡ Hola, chicos del scripting! ¿Cómo puedo habilitar y deshabilitar Archivos sin conexión?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Tecnologías relacionadas|[Identidad y acceso en Windows Server](../../identity/identity-and-access.md)<br>[Almacenamiento en Windows Server](../storage.md)<br>[Administración de acceso remoto y servidor](../../remote/index.md) |