---
title: Procedimientos recomendados del servidor de directivas de redes
description: En este tema se proporcionan los procedimientos recomendados para implementar y administrar el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: affb4785301bf71c18991c826dc17cb1cec6e633
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865224"
---
# <a name="network-policy-server-best-practices"></a>Procedimientos recomendados del servidor de directivas de redes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de los procedimientos recomendados para implementar y administrar el NPS del servidor de directivas de redes \( \) .

En las secciones siguientes se proporcionan procedimientos recomendados para los distintos aspectos de la implementación de NPS.

## <a name="accounting"></a>Control

A continuación se muestran los procedimientos recomendados para el registro de NPS.

Existen dos tipos de cuentas, o modos de registro, en NPS:

- Registro de eventos para NPS. Puede usar el registro de eventos para registrar eventos de NPS en los registros de eventos del sistema y de seguridad. Se usa principalmente para supervisar y solucionar los intentos de conexión.

- Registro de solicitudes de autenticación y cuentas de usuario. Puede registrar las solicitudes de cuentas y autenticación de usuarios en archivos de registro con formato de texto o formato de base de datos o bien registrarlas en un procedimiento almacenado en una base de datos de SQL Server 2000. El registro de solicitudes se utiliza principalmente para el análisis de la conexión y la facturación, y también es útil como herramienta de investigación de seguridad, lo que le proporciona un método para realizar un seguimiento de la actividad de un atacante.

Para hacer un uso más efectivo del registro de NPS:

- Active el registro \( inicialmente \) para los registros de autenticación y de cuentas. Modifique estas selecciones después de determinar qué es más apropiado para su entorno.

- Asegúrese de configurar el registro de eventos con una capacidad suficiente para mantener sus registros.

- Realice una copia de seguridad de todos los archivos de registro con regularidad porque no se pueden volver a crear cuando están dañados o eliminados.

- Use el atributo Class de RADIUS para registrar el uso y facilitar la identificación del departamento o usuario al que debe cargarse el uso. Aunque el atributo Class que se genera automáticamente es único para cada solicitud, es posible que se produzcan registros duplicados en los casos en que la respuesta al servidor de acceso se haya perdido y la solicitud se reenvíe. Es posible que tenga que eliminar solicitudes duplicadas de sus registros para realizar un seguimiento preciso del uso.

- Si los servidores de acceso a la red y los servidores proxy RADIUS envían periódicamente mensajes de solicitud de conexión ficticia a NPS para comprobar que el NPS está en línea, use la configuración de registro **ping User-Name** . Esta opción configura NPS para que rechace automáticamente estas solicitudes de conexión falsas sin procesarlas. Además, NPS no registra las transacciones que implican el nombre de usuario ficticio en los archivos de registro, lo que facilita la interpretación del registro de eventos.

- Deshabilitar el reenvío de notificaciones de NAS. Puede deshabilitar el reenvío de mensajes de inicio y detención de los servidores de acceso a la red (NAS) a los miembros de un grupo de servidores RADIUS remotos que está configurado en NPS. Para obtener más información, consulte [deshabilitar el reenvío de notificaciones de NAS](nps-disable-nas-notifications.md).

Para obtener más información, vea [configurar las cuentas del servidor de directivas de redes](nps-accounting-configure.md).

- Para ofrecer capacidad de conmutación por error y redundancia con el registro de SQL Server, coloque dos equipos que ejecuten SQL Server en subredes diferentes. Use el **Asistente para crear publicaciones** de SQL Server para configurar la replicación de base de datos entre los dos servidores. Para obtener más información, vea [SQL Server documentación técnica](/sql/sql-server/) y [replicación de SQL Server](/sql/relational-databases/replication/sql-server-replication).

## <a name="authentication"></a>Authentication

A continuación, se incluyen recomendaciones para la autenticación.

- Use métodos de autenticación basados en certificados, como el protocolo de autenticación extensible protegido \( PEAP \) y el protocolo de autenticación extensible \( EAP para la \) autenticación segura. No use métodos de autenticación de solo contraseña porque son vulnerables a diversos ataques y no son seguros. Para la autenticación inalámbrica segura, \- se recomienda usar PEAP MS \- CHAP V2, ya que el NPS demuestra su identidad a los clientes inalámbricos mediante el uso de un certificado de servidor, mientras que los usuarios demuestran su identidad con su nombre de usuario y contraseña.  Para obtener más información sobre el uso de NPS en la implementación inalámbrica, consulte [implementación de acceso inalámbrico autenticado en Password-Based 802.1 x](../../core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access.md).
- Implemente su propia CA de entidad \( \) de certificación con Active Directory &reg; servicios de Certificate Server \( ad CS \) cuando use métodos seguros de autenticación basados en certificados, como PEAP y EAP, que requieran el uso de un certificado de servidor en NPSs. También puede usar su entidad de certificación para realizar inscripciones de certificados de equipo y de usuario. Para obtener más información sobre la implementación de certificados de servidor en NPS y servidores de acceso remoto, consulte [implementación de certificados de servidor para implementaciones cableadas e inalámbricas de 802.1 x](../../core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments.md).

> [!IMPORTANT]
> El servidor de directivas de redes (NPS) no admite el uso de los caracteres ASCII extendidos en las contraseñas.

## <a name="client-computer-configuration"></a>Configuración del equipo cliente

A continuación, se incluyen recomendaciones para configurar el equipo cliente.

- Configure automáticamente todos los equipos cliente de 802.1 X del miembro de dominio mediante directiva de grupo. Para obtener más información, consulte la sección "configuración de directivas de red inalámbrica (IEEE 802,11)" en el tema [implementación de acceso inalámbrico](../../core-network-guide/cncg/wireless/e-wireless-access-deployment.md#bkmk_policies).

## <a name="installation-suggestions"></a>Sugerencias de instalación

A continuación se muestran los procedimientos recomendados para la instalación de NPS.

- Antes de instalar NPS, instale y pruebe cada uno de los servidores de acceso a la red mediante métodos de autenticación locales antes de configurarlos como clientes RADIUS en NPS.

- Después de instalar y configurar NPS, guarde la configuración mediante el comando de Windows PowerShell [Export-NpsConfiguration](/powershell/module/nps/export-npsconfiguration). Guarde la configuración de NPS con este comando cada vez que vuelva a configurar el NPS.

>[!CAUTION]
>- El archivo de configuración de NPS exportado contiene secretos compartidos sin cifrar para clientes RADIUS y miembros de grupos de servidores RADIUS remotos. Por este motivo, asegúrese de guardar el archivo en una ubicación segura.
>- El proceso de exportación no incluye la configuración de registro de Microsoft SQL Server en el archivo exportado. Si importa el archivo exportado a otro NPS, debe configurar manualmente el registro de SQL Server en el nuevo servidor.

## <a name="performance-tuning-nps"></a>Optimización del rendimiento de NPS

A continuación se muestran los procedimientos recomendados para optimizar el rendimiento de NPS.

- Para optimizar los tiempos de respuesta de autenticación y autorización de NPS y reducir el tráfico de red, instale NPS en un controlador de dominio.

- Cuando se utilizan nombres de entidad de seguridad universal \( UPN \) o dominios de windows Server 2008 y windows Server 2003, NPS usa el catálogo global para autenticar a los usuarios. Para minimizar el tiempo necesario para ello, instale NPS en un servidor de catálogo global o en un servidor que se encuentra en la misma subred que el servidor de catálogo global.

- Cuando tiene grupos de servidores RADIUS remotos configurados y, en las directivas de solicitud de conexión NPS, desactiva la casilla **registrar información de cuentas en los servidores en el siguiente grupo de servidores RADIUS remotos** , estos grupos siguen enviando \( mensajes de notificación de inicio y detención de NAS en el servidor de acceso a la red \) . Esto genera un tráfico de red innecesario. Para eliminar este tráfico, deshabilite el reenvío de notificaciones de NAS para servidores individuales en cada grupo de servidores RADIUS remotos. para ello, desactive la casilla **reenviar notificaciones de inicio y detención de red a este servidor** .

## <a name="using-nps-in-large-organizations"></a>Uso de NPS en organizaciones grandes

A continuación se muestran los procedimientos recomendados para el uso de NPS en organizaciones de gran tamaño.

- Si usa directivas de red para restringir el acceso a todos los grupos excepto a algunos, cree un grupo universal para todos los usuarios a los que desea permitir el acceso y, a continuación, cree una directiva de red que conceda acceso a este grupo universal. No incluya a todos los usuarios directamente en el grupo universal, especialmente si hay muchos usuarios en su red. En lugar de ello, cree grupos separados que pertenezcan al grupo universal y agregue usuarios a esos grupos.

- Siempre que sea posible, use nombres principales de usuario para hacer referencia a los usuarios. Un usuario puede tener el mismo nombre principal de usuario al margen del dominio al que pertenezca. Esta práctica proporciona la escalabilidad necesaria a las organizaciones que tienen un elevado número de dominios.

- Si instaló NPS del servidor \( de directivas \) de redes en un equipo que no sea un controlador de dominio y el NPS recibe un gran número de solicitudes de autenticación por segundo, puede mejorar el rendimiento de NPS aumentando el número de autenticaciones simultáneas permitidas entre el NPS y el controlador de dominio. Para obtener más información, consulte [aumentar las autenticaciones simultáneas procesadas por NPS](./nps-concurrent-auth.md).

## <a name="security-issues"></a>Problemas de seguridad

A continuación se muestran los procedimientos recomendados para reducir los problemas de seguridad.

Cuando administre un NPS de forma remota, no envíe datos confidenciales (por ejemplo, secretos o contraseñas compartidos) a través de la red en texto sin formato. Hay dos métodos recomendados para la administración remota de NPSs:

- Use Servicios de Escritorio remoto para tener acceso al NPS. Cuando se usa Servicios de Escritorio remoto, no se envían datos entre el cliente y el servidor. Solo la interfaz de usuario del servidor (por ejemplo, el escritorio del sistema operativo y la imagen de la consola de NPS) se envía al cliente de Servicios de Escritorio remoto, que se denomina Conexión a Escritorio remoto en Windows &reg; 10. El cliente envía la entrada del teclado y del mouse, que es procesada localmente por el servidor que tiene Servicios de Escritorio remoto habilitado. Cuando Servicios de Escritorio remoto usuarios inician sesión, solo pueden ver las sesiones de cliente individuales, que son administradas por el servidor y son independientes entre sí. Además, la Conexión a Escritorio remoto permite el cifrado de 128 bits entre cliente y servidor.

- Usar el protocolo de seguridad de Internet (IPsec) para cifrar datos confidenciales. Puede usar IPsec para cifrar la comunicación entre el NPS y el equipo cliente remoto que está usando para administrar NPS. Para administrar el servidor de forma remota, puede instalar el [herramientas de administración remota del servidor para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) en el equipo cliente. Después de la instalación, use Microsoft Management Console (MMC) para agregar el complemento NPS a la consola de.

>[!IMPORTANT]
>Solo puede instalar Herramientas de administración remota del servidor para Windows 10 en la versión completa de Windows 10 Professional o Windows 10 Enterprise.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
