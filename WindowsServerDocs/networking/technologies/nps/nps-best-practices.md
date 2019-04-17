---
title: Procedimientos recomendados de servidor de directivas de red
description: En este tema se proporciona procedimientos recomendados para implementar y administrar el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a9a68965d0d19504352ad67f7ab268b3d344fb2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-best-practices"></a>Procedimientos recomendados de servidor de directivas de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre los procedimientos recomendados para implementar y administrar \(NPS\) el servidor de directivas de red.

Las siguientes secciones proporcionan los procedimientos recomendados para diferentes aspectos de la implementación de NPS.

## <a name="accounting"></a>Administración de cuentas

Siguiente es los procedimientos recomendados para el registro de NPS.

Hay dos tipos de contabilidad o cierre de sesión, en NPS:

- Registro de eventos para NPS. Puedes usar el registro de eventos para registrar los eventos NPS en los registros de eventos de sistema y seguridad. Esto se usa principalmente para la auditoría y solución de problemas de intentos de conexión.

- Registro de autenticación de usuario y las solicitudes de cuentas. Puede iniciar las solicitudes de autenticación y cuentas de usuario en archivos de registro en formato de texto o base de datos, o puedes iniciar sesión a un procedimiento almacenado en una base de datos de SQL Server 2000. Registro de solicitud se usa principalmente para fines de análisis y la facturación de conexión y también es útil como una herramienta de investigación de seguridad, proporcionar un método de seguimiento de la actividad de un atacante.

Para hacer un uso más eficaz de registro NPS:

- Activar el registro \(initially\) para la autenticación y registros de contabilidad. Modificar estas selecciones después de determinar lo que es apropiado para el entorno.

- Asegúrate de que el registro de eventos está configurado con una capacidad suficiente para mantener los registros.

- Hacer una copia de todos los archivos de registro de forma regular porque no se puede volver a crearse cuando están dañados o eliminados.

- Usar el atributo de clase de RADIUS un seguimiento del uso tanto a simplificar la identificación del departamento o usuario que se cobrará por uso. Aunque el atributo de clase generado automáticamente es único para cada solicitud, podrían existir registros duplicados cuando se pierde la respuesta al servidor de acceso y se vuelve a enviar la solicitud. Es posible que debas eliminar solicitudes duplicadas de los registros de forma precisa un seguimiento del uso.

- Si tus servidores de acceso de red y servidores proxy RADIUS periódicamente envían mensajes de solicitud de conexión ficticia a NPS para comprobar que el servidor NPS está en línea, utilice el **ping el nombre de usuario** configuración del registro. Esta configuración configura NPS para rechazar automáticamente estas solicitudes de conexión false sin procesarlos. Además, NPS no registra las transacciones relacionadas con el nombre de usuario ficticio en los archivos de registro, lo que facilita la interpretar el registro de eventos.

- Deshabilitar el reenvío de Notification NAS. Puedes deshabilitar el reenvío de inicio y detención mensajes desde servidores de acceso de red (NAS) a los miembros de un servidor remoto RADIUS de grupo que está configurado en NPS. Para obtener más información, consulta [deshabilitar reenvío de Notification NAS](nps-disable-nas-notifications.md).

Para obtener más información, consulta [configurar red directiva de servidor de contabilidad](nps-accounting-configure.md).

- Para proporcionar la conmutación por error y redundancia con el registro de SQL Server, coloca dos equipos que ejecutan SQL Server en subredes diferentes. Usa SQL Server **asistente** para configurar la replicación de base de datos entre los dos servidores. Para obtener más información, consulta [documentación técnica de SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) y [duplicación de SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticación

Siguiente es los procedimientos recomendados para la autenticación.

- Usar métodos de autenticación basada en certificados como protocolo de autenticación Extensible protegido \(PEAP\) y protocolo de autenticación Extensible \(EAP\) para la autenticación segura. No utilice los métodos de autenticación solo contraseña porque son vulnerables a una variedad de ataques y no son seguras. Para la autenticación inalámbrica segura, PEAP\-MS\-CHAPv2 se recomienda usar, porque el servidor NPS demuestra su identidad a los clientes inalámbricos con un certificado de servidor, mientras que los usuarios demostrar su identidad con su nombre de usuario y contraseña.  Para obtener más información sobre cómo usar NPS en tu implementación inalámbrica, consulta [basada en contraseña implementar 802.1X X autenticados acceso inalámbrico](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Implementar tu propia entidad de certificación \(CA\) con Active Directory&reg; servicios de certificados \(AD CS\) cuando usas métodos de autenticación segura basada en certificados, como PEAP, EAP, que requieren el uso de un certificado de servidor en los servidores NPS. También puedes usar la entidad de certificación para inscribir los certificados de equipo y certificados de usuario. Para obtener más información sobre cómo implementar los certificados de servidor a servidores NPS y acceso remoto, vea [implementar certificados de servidor para implementaciones de conexión inalámbrica y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configuración del equipo cliente

Siguiente es los procedimientos recomendados para la configuración del equipo cliente.

- Configurar automáticamente todo el miembro de dominio 802.1X X equipos cliente mediante la directiva de grupo. Para obtener más información, consulta la sección "Configurar inalámbrica (IEEE 802.11) de la red directivas" en el tema [implementación de acceso inalámbrico](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Sugerencias de instalación

Siguiente es los procedimientos recomendados para la instalación de NPS.

- Antes de instalar NPS, instalar y probar cada uno de los servidores de acceso de red usando los métodos de autenticación local antes de configurar como clientes RADIUS en NPS.

- Después de instalar y configurar NPS, guardar la configuración con el comando de Windows PowerShell [exportación NpsConfiguration](https://technet.microsoft.com/en-us/library/jj872749.aspx). Guardar la configuración de NPS con este comando cada vez que volver a configurar el servidor NPS.

>[!CAUTION]
>- El archivo de configuración de NPS exportado contiene secretos compartidos sin cifrar para los clientes RADIUS y los miembros de grupos de servidores RADIUS remotos. Por este motivo, asegúrate de que guardes el archivo en una ubicación segura.
>- El proceso de exportación no incluye la configuración de registro de Microsoft SQL Server en el archivo exportado. Si importación el archivo exportado a otro servidor NPS, debes configurar manualmente registro de SQL Server en el servidor nuevo.

## <a name="performance-tuning-nps"></a>NPS el ajuste del rendimiento

Siguiente es los procedimientos recomendados para NPS de optimización del rendimiento.

- Para optimizar los tiempos de respuesta de autenticación y autorización de NPS y minimizar el tráfico de red, instala NPS en un controlador de dominio.

- Cuando se usan nombres de entidad de seguridad universal \(UPNs\) o dominios de Windows Server 2008 y Windows Server 2003, NPS usa el catálogo global para autenticar usuarios. Para minimizar el tiempo que tarda en hacer esto, instala NPS en un servidor de catálogo global o un servidor que se encuentra en la misma subred que el servidor de catálogo global.

- Cuando tienes grupos de servidores RADIUS remotos configurados y, en las directivas de solicitud de conexión de NPS, desactiva la **registra información de cuentas en los servidores en los siguientes grupos de servidores RADIUS remotos** casilla de verificación, estos grupos aún se envían el inicio \(NAS\) de servidor de acceso de red y dejar de mensajes de notificación. Esto crea tráfico de red innecesario. Para eliminar este tráfico, deshabilite el reenvío de notification NAS para servidores individuales en cada grupo de servidores RADIUS remotos, desactive la **reenviar el inicio de la red y dejar de recibir notificaciones a este servidor** casilla de verificación.

## <a name="using-nps-in-large-organizations"></a>Uso de NPS en las grandes empresas

Siguiente es los procedimientos recomendados para usar NPS en las grandes empresas.

- Si estás usando las directivas de red para restringir el acceso para determinados grupos, crear un grupo universal para todos los usuarios que quieres permitir el acceso y, a continuación, crear una directiva de red que conceda acceso a este grupo universal. No coloques todos los usuarios directamente en el grupo universal, especialmente si tienes un gran número de ellos en la red. En su lugar, crea grupos independientes que son miembros del grupo universal y agregar usuarios a esos grupos.

- Usar un nombre principal de usuario para hacer referencia a los usuarios siempre que sea posible. Un usuario puede tener el mismo nombre principal de usuario independientemente de la pertenencia a un dominio. Este procedimiento proporciona la escalabilidad que puede que tengas en organizaciones que tienen un gran número de dominios.

- Si instalaste \(NPS\) el servidor de directivas de red en un equipo que no sea un controlador de dominio y el servidor NPS recibe un gran número de solicitudes de autenticación por segundo, puede mejorar el rendimiento de NPS aumentando el número de autenticaciones simultáneas que se permiten entre el servidor NPS y el controlador de dominio. Para obtener más información, consulta 

## <a name="security-issues"></a>Problemas de seguridad

Siguiente es los procedimientos recomendados para reducir problemas de seguridad.

Al administrar un servidor NPS de forma remota, no envíe datos confidenciales (por ejemplo, contraseñas o secretos compartidos) en la red en texto sin formato. Hay dos métodos recomendados para la administración remota de servidores NPS:

- Usar servicios de escritorio remoto para obtener acceso al servidor NPS. Al usar servicios de escritorio remoto, no se envían datos entre el cliente y servidor. La interfaz de usuario del servidor (por ejemplo, el escritorio del sistema operativo y la imagen de la consola NPS) se envía al cliente de servicios de escritorio remoto, que se denomina conexión a Escritorio remoto en Windows&reg; 10. El cliente envía teclado y mouse, que procesa localmente mediante el servidor que tiene servicios de escritorio remoto habilitado. Cuando los usuarios de servicios de escritorio remoto iniciar sesión, pueden ver solo sus sesiones de cliente individual, que administren por el servidor y son independientes de ellos. Además, la conexión a Escritorio remoto proporciona cifrado de 128 bits entre el cliente y servidor.

- Usar la seguridad de protocolo de Internet (IPsec) para cifrar datos confidenciales. Puedes usar IPsec para cifrar la comunicación entre el servidor NPS y el equipo cliente remoto que se usa para administrar NPS. Para administrar el servidor de forma remota, puedes instalar el [remoto Server Administration Tools para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) en el equipo cliente. Después de la instalación, usar Microsoft Management Console (MMC) para agregar el complemento de servidor NPS a la consola.

>[!IMPORTANT]
>Puedes instalar remoto Server Administration Tools para Windows 10 solo en la versión completa de Windows 10 Professional o Windows 10 Enterprise.

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).

