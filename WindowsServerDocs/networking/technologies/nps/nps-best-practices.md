---
title: Procedimientos recomendados del servidor de directivas de redes
description: En este tema proporciona prácticas recomendadas para implementar y administrar el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6cbd606edec06a80767ee997ef6a1397b66b7843
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834236"
---
# <a name="network-policy-server-best-practices"></a>Procedimientos recomendados del servidor de directivas de redes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre los procedimientos recomendados para implementar y administrar el servidor de directivas de red \(NPS\).

Las secciones siguientes proporcionan procedimientos recomendados para los distintos aspectos de la implementación de NPS.

## <a name="accounting"></a>Cuentas

A continuación es los procedimientos recomendados para el registro de NPS.

Hay dos tipos de cuentas, o el registro, en NPS:

- Registro de eventos de NPS. Puede usar el registro de eventos para registrar eventos NPS en los registros de eventos del sistema y seguridad. Esto se utiliza principalmente para la auditoría y solución de problemas de intentos de conexión.

- Registro de autenticación de usuario y las solicitudes de cuentas. Puede registrar las solicitudes de autenticación y cuentas de usuario en archivos de registro en formato de texto o base de datos, o bien registrarlas en un procedimiento almacenado en una base de datos de SQL Server 2000. Registro de solicitudes se utiliza principalmente para fines de facturación y análisis de la conexión y también es útil como herramienta de investigación de seguridad, proporcionar un método de seguimiento de la actividad de un atacante.

Para hacer un uso más eficaz de registro NPS:

- Activar el registro \(inicialmente\) para la autenticación y los registros de cuentas. Modifique estas selecciones después de determinar lo que sea adecuado para su entorno.

- Asegúrese de que el registro de eventos está configurado con una capacidad suficiente para mantener sus registros.

- Copia de seguridad de todos los archivos de registro de forma regular porque no se puede volver a crearse cuando están dañados o eliminados.

- Utilice el atributo Class de RADIUS para registrar el uso y facilitar la identificación del departamento o usuario para cargar el uso. Aunque el atributo de clase generado automáticamente es único para cada solicitud, los registros duplicados pueden existir en los casos en que se pierde la respuesta al servidor de acceso y se vuelve a enviar la solicitud. Es posible que deba eliminar solicitudes duplicadas de los registros para realizar un seguimiento preciso de uso.

- Si los servidores de acceso de red y servidores proxy RADIUS envían periódicamente mensajes de solicitud ficticio de conexión a NPS para comprobar que el NPS está en línea, use el **hacer ping en nombre de usuario** configuración del registro. Este valor configura NPS para rechazar automáticamente las solicitudes de conexión false sin procesarlos. Además, NPS no registra las transacciones relacionadas con el nombre de usuario ficticio en los archivos de registro, lo que hace más fácil de interpretar el registro de eventos.

- Deshabilitar el reenvío de notificaciones de NAS. Puede deshabilitar el reenvío de inicio y detención mensajes desde los servidores de acceso de red (NAS) a los miembros de un servidor RADIUS remoto de grupo que se configura en NPS. Para obtener más información, consulte [deshabilitar el reenvío de notificaciones de NAS](nps-disable-nas-notifications.md).

Para obtener más información, consulte [configurar red directiva de servidor de contabilidad](nps-accounting-configure.md).

- Para proporcionar redundancia y conmutación por error con el registro de SQL Server, coloque dos equipos que ejecuten SQL Server en subredes diferentes. Usar SQL Server **Asistente para crear la publicación** para configurar la replicación de base de datos entre los dos servidores. Para obtener más información, consulte [documentación técnica de SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) y [replicación de SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticación

A continuación es los procedimientos recomendados para la autenticación.

- Usar métodos de autenticación basada en certificados, como el protocolo de autenticación Extensible protegido \(PEAP\) y protocolo de autenticación Extensible \(EAP\) para una autenticación segura. No use métodos de autenticación de contraseña solo porque son vulnerables a diferentes ataques y no son seguras. Para la autenticación inalámbrica segura, mediante PEAP\-MS\-CHAP v2 se recomienda porque el NPS demuestra su identidad a los clientes inalámbricos mediante el uso de un certificado de servidor, mientras que los usuarios demostrar su identidad con su nombre de usuario y contraseña.  Para obtener más información sobre el uso de NPS en la implementación inalámbrica, consulte [basado en contraseña implementar autentica el acceso inalámbrico 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Implementar su propia entidad de certificación \(CA\) con Active Directory&reg; servicios de Certificate Server \(AD CS\) cuando se usan métodos de autenticación segura basada en certificados, como PEAP y EAP, que requieren el uso de un certificado de servidor en NPSs. También puede usar la entidad de certificación para inscribir certificados de equipo y los certificados de usuario. Para obtener más información sobre la implementación de certificados de servidor en servidores NPS y acceso remoto, consulte [implementar certificados de servidor para las implementaciones inalámbricas y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configuración del equipo cliente

Siguientes son los procedimientos recomendados para la configuración del equipo cliente.

- Configurar automáticamente todos los miembros de dominio 802.1X X de los equipos cliente mediante la directiva de grupo. Para obtener más información, consulte la sección "Configuración inalámbrica (IEEE 802.11) de red de directivas" en el tema [implementación del acceso inalámbrico](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Sugerencias de instalación

A continuación es los procedimientos recomendados para la instalación de NPS.

- Antes de instalar NPS, instale y pruebe cada uno de los servidores de acceso de red mediante métodos de autenticación local antes de configurar como clientes RADIUS en NPS.

- Después de instalar y configurar NPS, guarde la configuración mediante el comando de Windows PowerShell [exportación NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx). Guarde la configuración de NPS con este comando cada vez que vuelva a configurar el NPS.

>[!CAUTION]
>- El archivo de configuración de NPS exportado contiene secretos compartidos sin cifrar para clientes RADIUS y miembros de grupos de servidores RADIUS remotos. Por este motivo, asegúrese de que guarde el archivo en una ubicación segura.
>- El proceso de exportación no incluye la configuración del registro de Microsoft SQL Server en el archivo exportado. Si importa el archivo exportado a otro NPS, debe configurar manualmente el registro de SQL Server en el nuevo servidor.

## <a name="performance-tuning-nps"></a>NPS de optimización del rendimiento

A continuación es las prácticas recomendadas para NPS de optimización del rendimiento.

- Para optimizar los tiempos de respuesta de autenticación y autorización de NPS y minimizar el tráfico de red, instale NPS en un controlador de dominio.

- Cuando los nombres de entidad de seguridad universal \(UPN\) o se usan los dominios de Windows Server 2008 y Windows Server 2003, NPS usa el catálogo global para autenticar a los usuarios. Para minimizar el tiempo necesario para hacer esto, instale NPS en un servidor de catálogo global o un servidor que se encuentra en la misma subred que el servidor de catálogo global.

- Cuando tiene grupos de servidores RADIUS remotos configurados y, en directivas de solicitud de conexión de NPS, desactive el **registrar información de cuentas en los servidores en el siguiente grupo de servidores RADIUS remotos** casilla de verificación, estos grupos son todavía envía el servidor de acceso de red \(NAS\) iniciar y detener los mensajes de notificación. Esto crea el tráfico de red innecesario. Para eliminar este tráfico, deshabilite el reenvío de notificaciones de NAS para servidores individuales en cada grupo de servidores RADIUS remoto desactivando la **reenviar el inicio de la red y dejar de recibir notificaciones a este servidor** casilla de verificación.

## <a name="using-nps-in-large-organizations"></a>Uso de NPS en organizaciones de gran tamaño

A continuación es los procedimientos recomendados para el uso de NPS en organizaciones grandes.

- Si está usando directivas de red para restringir el acceso a determinados grupos, cree un grupo universal para todos los usuarios para los que desea permitir el acceso y, a continuación, crear una directiva de red que se concede acceso a este grupo universal. No coloque todos los usuarios directamente en el grupo universal, especialmente si tiene un gran número de ellos en la red. En su lugar, cree grupos separados que son miembros del grupo universal y agregar usuarios a esos grupos.

- Use un nombre principal de usuario para hacer referencia a los usuarios siempre que sea posible. Un usuario puede tener el mismo nombre principal de usuario, independientemente de la pertenencia al dominio. Esta práctica proporciona la escalabilidad que puede ser necesaria en las organizaciones con un gran número de dominios.

- Si ha instalado el servidor de directivas de red \(NPS\) en un equipo que no sea un dominio de controlador y el NPS recibe un gran número de solicitudes de autenticación por segundo, puede mejorar el rendimiento de NPS si aumenta el número de autenticaciones simultáneas permitidas entre el NPS y el controlador de dominio. Para obtener más información, consulte 

## <a name="security-issues"></a>Problemas de seguridad

Siguientes son los procedimientos recomendados para reducir los problemas de seguridad.

Cuando se administra remotamente un NPS, no envíe datos importantes o confidenciales (por ejemplo, los secretos compartidos o contraseñas) a través de la red en texto sin formato. Hay dos métodos recomendados para la administración remota de NPSs:

- Usar servicios de escritorio remoto para tener acceso a NPS. Cuando usa servicios de escritorio remoto, los datos no se envían entre cliente y servidor. La interfaz de usuario del servidor (por ejemplo, el escritorio del sistema operativo y la imagen de la consola NPS) se envía al cliente de servicios de escritorio remoto, que se denomina conexión a Escritorio remoto en Windows&reg; 10. El cliente envía teclado y mouse de entrada, que son procesados localmente por el servidor que tiene servicios de escritorio remoto habilitado. Cuando los usuarios de servicios de escritorio remoto se inician sesión, pueden ver sólo sus sesiones de cliente individuales, que son administrados por el servidor y son independientes entre sí. Además, la conexión a Escritorio remoto proporciona cifrado de 128 bits entre cliente y servidor.

- Usar seguridad de protocolo Internet (IPsec) para cifrar datos confidenciales. Puede utilizar IPsec para cifrar la comunicación entre el NPS y el equipo cliente remoto que está usando para administrar el NPS. Para administrar el servidor de forma remota, puede instalar el [remoto Server Administration Tools for Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) en el equipo cliente. Después de la instalación, utilice Microsoft Management Console (MMC) para agregar el complemento NPS en la consola.

>[!IMPORTANT]
>Puede instalar remoto Server Administration Tools for Windows 10 solo en la versión completa de Windows 10 Professional o Windows 10 Enterprise.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).

