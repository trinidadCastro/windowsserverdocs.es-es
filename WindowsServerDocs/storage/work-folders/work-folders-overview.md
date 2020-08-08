---
ms.assetid: c91c7196-ee0d-4856-8cfb-4c38494ccf1f
title: Introducción a carpetas de trabajo
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 06/15/2020
description: 'Información general de las carpetas de trabajo: un rol de servidor en Windows Server que proporciona una manera coherente de que los usuarios tengan acceso a los archivos de trabajo desde equipos y dispositivos.'
ms.openlocfilehash: adc03d9bcb4289896b996984ebb53b185008f3fb
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994108"
---
# <a name="work-folders-overview"></a>Introducción a carpetas de trabajo

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

En este tema se describen las carpetas de trabajo, un servicio de función para los servidores de archivos que ejecutan Windows Server y que proporcionan una manera coherente de que los usuarios tengan acceso a sus archivos de trabajo desde sus equipos y dispositivos.

Si busca descargar o usar carpetas de trabajo en Windows 10, Windows 7 o en un dispositivo iOS o Android, consulte lo siguiente:

- [Carpetas de trabajo para Windows 10](https://support.microsoft.com/help/12370/windows-10-work-folders)
- [Carpetas de trabajo para Windows 7 (descarga de 64 bits)](https://www.microsoft.com/download/details.aspx?id=42558)
- [Carpetas de trabajo para Windows 7 (descarga de 32 bits)](https://www.microsoft.com/download/details.aspx?id=42559)
- [Carpetas de trabajo para iOS](https://itunes.apple.com/app/work-folders/id950878067)
- [Carpetas de trabajo para Android](https://play.google.com/store/apps/details?id=com.microsoft.workfolders)

## <a name="role-description"></a>Descripción del rol

 Con Carpetas de trabajo, los usuarios pueden almacenar y tener acceso a archivos de trabajo en equipos y dispositivos, lo que suele denominarse Traer su propio dispositivo (BYOD), así como a equipos corporativos. Los usuarios obtienen una ubicación adecuada para almacenar los archivos de trabajo y pueden tener acceso a ellos desde cualquier lugar. Las organizaciones mantienen el control sobre los datos corporativos almacenando los archivos en servidores de archivos administrados centralmente y, de manera opcional, especificando directivas de dispositivo de usuario como contraseñas de cifrado y de bloqueo de pantalla.

 Las carpetas de trabajo se pueden implementar con implementaciones existentes de redirección de carpetas, Archivos sin conexión y carpetas principales. Carpetas de trabajo almacena los archivos de usuario en una carpeta en el servidor que recibe el nombre de *recurso compartido de sincronización*. Puede especificar una carpeta que ya contiene datos de usuario, lo que le permite adoptar carpetas de trabajo sin migrar los servidores y los datos, o bien hacer que la solución existente se ponga en fase inmediata.

## <a name="practical-applications"></a>Aplicaciones prácticas

 Los administradores pueden usar carpetas de trabajo para proporcionar a los usuarios acceso a sus archivos de trabajo a la vez que mantienen un almacenamiento y control centralizados sobre los datos de la organización. Algunas aplicaciones específicas para carpetas de trabajo incluyen:

-   Proporcionar un único punto de acceso a los archivos de trabajo desde los equipos y dispositivos personales y de trabajo de un usuario

-   Acceder a archivos de trabajo sin conexión y, a continuación, sincronizar con el servidor de archivos central cuando el equipo o dispositivo siguiente tenga conectividad a Internet o a la intranet

-   Implementación con implementaciones existentes de redirección de carpetas, Archivos sin conexión y carpetas principales

-   Usar tecnologías de administración de servidores de archivos existentes, como clasificación de archivos y cuotas de carpetas, para administrar datos de usuario

-   Especificar directivas de seguridad para indicar a los equipos y dispositivos del usuario que cifren carpetas de trabajo y utilicen una contraseña de pantalla de bloqueo

-   Usar clústeres de conmutación por error con carpetas de trabajo para proporcionar una solución de alta disponibilidad

## <a name="important-functionality"></a>Funcionalidad importante

 Carpetas de trabajo incluye la siguiente funcionalidad.

| Funcionalidad | Disponibilidad | Descripción |
| ------------------- | ------------------ | ----------------- |
| Servicio de rol carpetas de trabajo en Administrador del servidor | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 | Los servicios de archivos y almacenamiento proporcionan una manera de configurar los recursos compartidos de sincronización (carpetas que almacenan los archivos de trabajo del usuario), supervisa las carpetas de trabajo y administra los recursos compartidos de sincronización y el acceso de usuario |
| Cmdlets de carpetas de trabajo | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 | Un módulo de Windows PowerShell que contiene cmdlets completos para administrar servidores de carpetas de trabajo |
| Integración de carpetas de trabajo con Windows | Windows 10<p> Windows 8.1<p> Windows RT 8.1<p> Windows 7 (se requiere descarga) | Carpetas de trabajo proporciona la siguiente funcionalidad en equipos Windows:<p> -Un elemento del panel de control que configura y supervisa carpetas de trabajo<br />-Integración del explorador de archivos que facilita el acceso a los archivos de las carpetas de trabajo<br />-Un motor de sincronización que transfiere archivos hacia y desde un servidor de archivos central y maximiza la duración de la batería y el rendimiento del sistema |
| Aplicación carpetas de trabajo para dispositivos | Android<p> Apple iPhone y iPad® | Aplicación que permite a los dispositivos populares acceder a archivos en carpetas de trabajo. |

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

En la tabla siguiente se describen algunos de los principales cambios en las carpetas de trabajo.

| Característica/funcionalidad | ¿Nueva o actualizada? | Descripción |
| ---------------------------- | --------------------- | ----------------- |
| Registro mejorado | Novedades de Windows Server 2019 | Los registros de eventos del servidor de carpetas de trabajo se pueden usar para supervisar la actividad de sincronización e identificar los usuarios que no superen las sesiones de sincronización. Use el ID. de evento 4020 en el registro de eventos Microsoft-Windows-SyncShare/Operational para identificar qué usuarios están produciendo errores en las sesiones de sincronización. Use el ID. de evento 7000 y el ID. de evento 7001 en el registro de eventos Microsoft-Windows-SyncShare/Reporting para supervisar los usuarios que están completando correctamente las sesiones de sincronización de carga y descarga. |
| Contadores de rendimiento | Novedades de Windows Server 2019 | Se agregaron los siguientes contadores de rendimiento: bytes descargados/seg., bytes cargados/seg., usuarios conectados, archivos descargados/s, archivos cargados/seg., usuarios con detección de cambios, solicitudes entrantes/s y solicitudes pendientes. |
| Rendimiento de servidor mejorado | Actualizado en Windows Server 2019 | Se realizaron mejoras de rendimiento para controlar más usuarios por servidor. El límite por servidor varía y se basa en el número de archivos y la renovación de archivos. Para determinar el límite por servidor, los usuarios se deben agregar al servidor en fases. |
| Acceso a archivos a petición | Agregado a la versión 1803 de Windows 10 | Permite ver y tener acceso a todos los archivos. Puede controlar qué archivos se almacenan en el equipo y están disponibles sin conexión. El resto de los archivos siempre están visibles y no ocupan espacio en el equipo, pero necesita conectividad con el servidor de archivos de carpetas de trabajo para tener acceso a ellos. |
| Compatibilidad con Azure AD proxy de aplicación | Agregado a Windows 10, versión 1703, Android, iOS | Los usuarios remotos pueden acceder de forma segura a sus archivos en el servidor de carpetas de trabajo mediante Azure AD proxy de aplicación. |
| Replicación de cambios más rápida | Actualizado en Windows 10 y Windows Server 2016 | En Windows Server 2012 R2, cuando los cambios efectuados en el archivo se sincronizan con el servidor de Carpetas de trabajo, no se notifica el cambio a los clientes y estos esperan hasta 10 minutos para obtener la actualización. Cuando se usa Windows Server 2016, el servidor de carpetas de trabajo notifica inmediatamente a los clientes de Windows 10 y los cambios de archivo se sincronizan inmediatamente. Esta funcionalidad es nueva en Windows Server 2016 y requiere un cliente de Windows 10. Si usa un cliente anterior o el servidor de carpetas de trabajo es Windows Server 2012 R2, el cliente seguirá sondeando cada 10 minutos en busca de cambios. |
| Integrado con Windows Information Protection (WIP) | Agregado a la versión 1607 de Windows 10 | Si un administrador implementa WIP, las carpetas de trabajo pueden aplicar la protección de datos mediante el cifrado de los datos en el equipo. El cifrado utiliza una clave asociada con el identificador de empresa, que se puede borrar de forma remota mediante un paquete de administración de dispositivos móviles compatible, como Microsoft Intune. |

## <a name="software-requirements"></a>Requisitos de software

Carpetas de trabajo presenta los siguientes requisitos de software en cuanto a servidores de archivos e infraestructura de red:

-   Un servidor que ejecute Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 para hospedar recursos compartidos de sincronización con archivos de usuario

-   Un volumen con formato de sistema de archivos NTFS para almacenar archivos de usuario

-   Para aplicar directivas de contraseña en equipos con Windows 7, debe usar las directivas de contraseña de directiva de grupo. También debe excluir los equipos con Windows 7 de las directivas de contraseña de Carpetas de trabajo (si es que los usa).

-   Un certificado de servidor para cada servidor de archivos que vaya a hospedar carpetas de trabajo. Estos certificados deben provienen de una entidad de certificación (CA) que sea de confianza para los usuarios, idealmente una CA pública.

-   Opta Un bosque de Active Directory Domain Services con las extensiones de esquema en Windows Server 2012 R2 para admitir automáticamente la referencia de equipos y dispositivos al servidor de archivos correcto al usar varios servidores de archivos.

Existen más requisitos para permitir que los usuarios sincronicen a través de Internet:

-   La capacidad de hacer que un servidor sea accesible desde Internet mediante la creación de reglas de publicación en el proxy inverso o la puerta de enlace de red de la organización.

-   Opta Un nombre de dominio registrado públicamente y la capacidad de crear más registros DNS públicos para el dominio

-   (Opcional) Una infraestructura de Servicios de federación de Active Directory (AD FS) cuando se use la autenticación de AD FS.

Carpetas de trabajo presenta los siguientes requisitos de software relativos a los equipos cliente:

-   Los equipos y dispositivos deben ejecutar uno de los siguientes sistemas operativos:

    -   Windows 10

    -   Windows 8.1

    -   Windows RT 8.1

    -   Windows 7

    -   Android 4,4 KitKat y versiones posteriores

    -   iOS 10.2 y versiones posteriores

-   Los equipos con Windows 7 deben ejecutar una de las siguientes ediciones de Windows:

    -   Windows 7 Professional

    -   Windows 7 Ultimate

    -   Windows 7 Enterprise

-   Los equipos con Windows 7 deben estar Unidos al dominio de su organización (no pueden unirse a un grupo de trabajo).

-   Suficiente espacio disponible en una unidad local con formato NTFS para almacenar todos los archivos de usuario en carpetas de trabajo, además de 6 GB adicionales de espacio disponible si carpetas de trabajo se encuentra en la unidad del sistema, como de forma predeterminada. Carpetas de trabajo usa la siguiente ubicación de forma predeterminada **%USERPROFILE%\Work Folders**

     No obstante, esta ubicación se puede modificar durante la instalación (se pueden usar ubicaciones como tarjetas microSD y unidades USB con el formato del sistema de archivos NTFS, pero cabe recordar que la sincronización se detendrá si se extraen las unidades).

     El tamaño máximo de los archivos individuales es de 10 GB de forma predeterminada. No existe límite de almacenamiento por usuario, si bien los administradores pueden hacer uso de la función de cuotas del Administrador de recursos del servidor de archivos para implementar cuotas.

-   Carpetas de trabajo no admite revertir el estado de la máquina virtual de las máquinas virtuales cliente. realice en su lugar operaciones de copia de seguridad y restauración desde la máquina virtual cliente, ya sea por medio de Copia de seguridad de imagen del sistema o de cualquier otra aplicación de copia de seguridad.

## <a name="work-folders-compared-to-other-sync-technologies"></a>Carpetas de trabajo en comparación con otras tecnologías de sincronización

En la tabla siguiente se describe cómo se colocan varias tecnologías de Microsoft Sync y cuándo se deben usar.

| | Carpetas de trabajo | Archivos sin conexión | OneDrive para la Empresa | OneDrive |
| - | ------------------ | ------------------- | -------------------------- | -------------- |
| **Resumen de tecnología** | Sincroniza los archivos que se almacenan en un servidor de archivos con equipos y dispositivos | Sincroniza los archivos que se almacenan en un servidor de archivos con equipos que tienen acceso a la red corporativa (se pueden reemplazar por carpetas de trabajo) | Sincroniza los archivos que se almacenan en Office 365 o en SharePoint con equipos y dispositivos dentro o fuera de una red corporativa, y proporciona la funcionalidad de colaboración de documentos. | Sincroniza los archivos personales que se almacenan en OneDrive con equipos, equipos Mac y dispositivos. |
| **Diseñado para proporcionar acceso de usuario a los archivos de trabajo** | Sí | Sí | Sí | No |
| **servicio en la nube** | None | None | Office 365 | Microsoft OneDrive |
| **Servidores de la red interna** | Servidores de archivos que ejecutan Windows Server 2012 R2, Windows Server 2016 y Windows Server 2019 | Servidores de archivos | SharePoint Server (opcional) | Ninguno |
| **Clientes compatibles** | Equipos, iOS y Android | Equipos en una red corporativa o conectados a través de DirectAccess, VPN u otras tecnologías de acceso remoto | PC, iOS, Android, Windows Phone | PC, equipos Mac, Windows Phone, iOS y Android |

> [!NOTE]
>  Además de las tecnologías de sincronización enumeradas en la tabla anterior, Microsoft ofrece otras tecnologías de replicación, como Replicación DFS, que está diseñada para la replicación de servidor a servidor y BranchCache, que está diseñada como una tecnología de aceleración de WAN de sucursales. Para obtener más información, vea [espacios de nombres DFS y replicación DFS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)) y [información general sobre BranchCache](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831696(v=ws.11)) .

## <a name="server-manager-information"></a>Información sobre el Administrador del servidor

Carpetas de trabajo forma parte del rol servicios de archivos y almacenamiento. Puede instalar carpetas de trabajo mediante el Asistente para agregar roles y características o el `Install-WindowsFeature` cmdlet. Ambos métodos realizan lo siguiente:

-   Agrega la página **carpetas de trabajo** a servicios de **archivos y almacenamiento** en Administrador del servidor

-   Instala el servicio de recursos compartidos de sincronización de Windows, que usa Windows Server para hospedar los recursos compartidos de sincronización

-   Instala el módulo SyncShare de Windows PowerShell para administrar carpetas de trabajo en el servidor.

## <a name="interoperability-with-windows-azure-virtual-machines"></a>Interoperabilidad con máquinas virtuales de Windows Azure

 Puede ejecutar este servicio de función de Windows Server en una máquina virtual de Windows Azure. Este escenario se ha probado con Windows Server 2012 R2, Windows Server 2016 y Windows Server 2019.

Para obtener información acerca de cómo empezar a trabajar con máquinas virtuales de Windows Azure, visite el [sitio web de Windows Azure](https://www.windowsazure.com/documentation/services/virtual-machines).

## <a name="see-also"></a>Consulte también

| Tipo de contenido | Referencias |
| ------------------ | ---------------- |
| **Evaluación del producto** | -   [Carpetas de trabajo para Android: publicado](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog)<br />-   [Carpetas de trabajo para iOS: versión de la aplicación de iPad](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog)<br />-   [Introducción a las carpetas de trabajo en Windows Server 2012 R2](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog)<br />-   [Introducción a las carpetas de trabajo](https://channel9.msdn.com/posts/Introduction-to-Work-Folders) (vídeo de Channel 9)<br />-   [Implementación del laboratorio de pruebas de carpetas de trabajo](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog)<br />-   [Carpetas de trabajo para Windows 7](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog) |
| **Implementación** | -   [Diseñar una implementación de carpetas de trabajo](plan-work-folders.md)<br />-   [Implementar carpetas de trabajo](deploy-work-folders.md)<br />-   [Implementación de carpetas de trabajo con AD FS y proxy de aplicación web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [Implementación de carpetas de trabajo con Azure AD proxy de aplicación](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)<br />- [Guía de migración de Archivos sin conexión (CSC) a carpetas de trabajo](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)<br />-   [Consideraciones de rendimiento para las implementaciones de carpetas de trabajo](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)<br />-   [Carpetas de trabajo para Windows 7 (descarga de 64 bits)](https://www.microsoft.com/download/details.aspx?id=42558)<br />-   [Carpetas de trabajo para Windows 7 (descarga de 32 bits)](https://www.microsoft.com/download/details.aspx?id=42559) |
| **Operaciones** | -   [Carpetas de trabajo de la aplicación de iPad: preguntas más frecuentes](https://windows.microsoft.com/windows/work-folders-ipad-faq) (para usuarios)<br />-   [Administración de certificados de carpetas de trabajo](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog)<br />-   [Supervisión de las implementaciones de carpetas de trabajo de Windows Server 2012 R2](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog)<br />-   [Cmdlets de SyncShare (carpetas de trabajo) en Windows PowerShell](/powershell/module/syncshare/?view=win10-ps)<br />-   [Tarjeta de referencia rápida de cmdlets de PowerShell para servicios de archivos y almacenamiento para Windows Server 2012 R2 Preview Edition](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) |
| **Solución de problemas** | -   [Windows Server 2012 R2: resolución de conflictos de puerto con sitios web y carpetas de trabajo de IIS](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (entrada de blog)<br />-   [Errores comunes en carpetas de trabajo](https://techcommunity.microsoft.com/t5/storage-at-microsoft/troubleshooting-work-folders-on-windows-client/ba-p/425627) |
| **Recursos de la comunidad** | -   [Foro de almacenamiento y servicios de archivos](/answers/topics/windows-server-storage.html)<br />-   [El equipo de almacenamiento en el blog de Microsoft-File Cabinet](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)<br />-   [Pregunte al blog del equipo de servicios de directorio](/archive/blogs/askds/) |
| **Tecnologías relacionadas** | -   [Almacenamiento en Windows Server 2016](../storage.yml)<br>-   [Servicios de archivos y almacenamiento](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831487(v=ws.11))<br />-   [Administrador de recursos de servidor de archivos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831701(v=ws.11))<br />-   [Redirección de carpetas, Archivos sin conexión y perfiles de usuario móviles](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh848267(v=ws.11))<br />-   [BranchCache](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831696(v=ws.11))<br />-   [Espacios de nombres DFS y Replicación DFS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)) |