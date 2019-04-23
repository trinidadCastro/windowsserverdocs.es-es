---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Protección de los controladores de dominio frente a ataques
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6be2899e85b68578518d9de1805c287367608163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863096"
---
# <a name="securing-domain-controllers-against-attack"></a>Protección de los controladores de dominio frente a ataques

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número tres: Si un intruso tiene acceso físico al equipo, ya no es su equipo.* - [10 leyes inmutables de seguridad (versión 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Los controladores de dominio proporcionan el almacenamiento físico de la base de datos de AD DS, además de proporcionar los servicios y datos que permiten a las empresas administrar de forma eficaz sus servidores, estaciones de trabajo, los usuarios y aplicaciones. Si se obtiene acceso con privilegios a un controlador de dominio por un usuario malintencionado, ese usuario puede modificar, dañar o destruir la base de datos de AD DS y, por extensión, todos los sistemas y las cuentas que son administradas por Active Directory.  
  
Porque los controladores de dominio pueden leer y escribir en cualquier valor en la base de datos de AD DS, poner en peligro un controlador de dominio significa que el bosque de Active Directory no puede nunca se consideren de confianza nuevo a menos que sea capaz de realizar la recuperación mediante una copia de seguridad buena conocida y a Cierre los huecos que permiten el riesgo en el proceso.  
  
Dependiendo de la preparación, herramientas y conocimientos, modificación o un daño irreparable incluso a AD DS de un atacante se puede completar la base de datos en cuestión de minutos a horas, días o semanas. Lo que no importa cuánto tiempo un atacante tiene privilegios de acceso a Active Directory, pero ¿cuánto tiempo el atacante está previsto incluir en el momento al tener acceso con privilegios se obtiene. Poner en peligro un controlador de dominio puede proporcionar la ruta de acceso más expedita propagación a gran escala de acceso o la ruta de acceso más directa para la destrucción de Active Directory, servidores miembro y estaciones de trabajo. Por este motivo, los controladores de dominio se deben proteger por separado y más estrictas que la infraestructura general de Windows.  

## <a name="physical-security-for-domain-controllers"></a>Seguridad física de los controladores de dominio

Esta sección proporciona información acerca de cómo proteger físicamente los controladores de dominio, si los controladores de dominio son físicos o máquinas virtuales, en las ubicaciones de centro de datos, las sucursales y ubicaciones remotas incluso con solo los controles de una infraestructura básica.  
  
### <a name="datacenter-domain-controllers"></a>Controladores de dominio del centro de datos  
  
#### <a name="physical-domain-controllers"></a>Controladores de dominio físicos

En los centros de datos, controladores de dominio físicos deben instalarse en bastidores seguros dedicados o jaulas que son independientes de la población general del servidor. Cuando sea posible, los controladores de dominio deben configurarse con los chips del módulo de plataforma segura (TPM) y todos los volúmenes en los servidores de controlador de dominio deben protegerse mediante cifrado de unidad BitLocker. Por lo general, BitLocker agrega una sobrecarga del rendimiento en porcentajes de un solo dígito, pero protege el directorio de ataques, incluso si se quitan los discos desde el servidor. BitLocker también puede ayudar a proteger los sistemas frente a ataques, como los rootkits porque la modificación de archivos de arranque hará que el servidor arrancar en modo de recuperación para que se pueden cargar los archivos binarios originales. Si un controlador de dominio está configurado para usar el software RAID, SCSI conectado en serie, el almacenamiento de SAN/NAS, o no se puede implementar los volúmenes dinámicos, BitLocker, se debe usar almacenamiento por lo que se ha conectado localmente (con o sin RAID de hardware) en los controladores de dominio siempre que es posible.  
  
#### <a name="virtual-domain-controllers"></a>Controladores de dominio virtuales 

Si decide implementar controladores de dominio virtuales, debe asegurarse de que los controladores de dominio se ejecutan en hosts físicos independientes que otras máquinas virtuales en el entorno. Si usa una plataforma de virtualización de terceros, considere la posibilidad de implementar controladores de dominio virtual en el servidor de Hyper-V en Windows Server 2012 o Windows Server 2008 R2, que proporciona una superficie de ataque mínima y se pueden administrar con los controladores de dominio que hospeda en lugar de que se administran con el resto de los hosts de virtualización. Si implementa System Center Virtual Machine Manager (SCVMM) para la administración de la infraestructura de virtualización, puede delegar la administración de los hosts físicos en residen las máquinas virtuales de controlador de dominio y los controladores de dominio ellos mismos a los administradores autorizados. También debe considerar el separar el almacenamiento de controladores de dominio virtuales para evitar que los administradores de almacenamiento de acceso a los archivos de máquina virtual.  
  
### <a name="branch-locations"></a>Ubicaciones de sucursales  
  
#### <a name="physical-domain-controllers-in-branches"></a>Controladores de dominio físicos en ramas

En ubicaciones en el que residen varios servidores pero no están protegidos físicamente en el grado en que se protegen los servidores de centro de datos, controladores de dominio físicos deben configurarse con chips TPM y cifrado de unidad BitLocker para todos los volúmenes del servidor. Si un controlador de dominio no puede almacenarse en un espacio bloqueado en las ubicaciones de sucursales, considere la posibilidad de implementar los RODC en esas ubicaciones.  
  
#### <a name="virtual-domain-controllers-in-branches"></a>Controladores de dominio virtual en ramas

Siempre que sea posible, debe ejecutar controladores de dominio virtuales en las sucursales en diferentes hosts físicos que las otras máquinas virtuales en el sitio. En las sucursales donde no se pueden ejecutar controladores de dominio virtuales en hosts físicos independientes del resto de la población de virtual server, debe implementar los chips TPM y cifrado de unidad BitLocker en los hosts en el que los controladores de dominio virtual se ejecuten como mínimo, y todos los hosts si es posible. Según el tamaño de la sucursal y la seguridad de los hosts físicos, considere la posibilidad de implementar los RODC en sucursales.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Ubicaciones remotas con espacio limitado y seguridad

Si su infraestructura incluye ubicaciones en que se puede instalar un único servidor físico, un servidor capaz de ejecutar las cargas de trabajo de virtualización debe instalarse en la ubicación remota, y debe configurarse el cifrado de unidad BitLocker para proteger todas volúmenes en el servidor. Una máquina virtual en el servidor debe ejecutar un RODC, con otros servidores que ejecutan como máquinas virtuales independientes en el host. Información acerca de cómo planear la implementación de RODC se proporciona en el [Guía de implementación y planeamiento de controladores de dominio de sólo lectura](https://go.microsoft.com/fwlink/?LinkID=135993). Para obtener más información sobre cómo implementar y proteger los controladores de dominio virtualizados, consulte [controladores de dominio en ejecución en Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) en el sitio Web de TechNet. Para obtener más instrucciones para la protección de Hyper-V, delegar la administración de máquinas virtuales y la protección de máquinas virtuales, consulte el [Guía de seguridad de Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) Acelerador de soluciones en el sitio Web de Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemas operativos de controlador de dominio

Debe ejecutar todos los controladores de dominio en la versión más reciente de Windows Server que se admite dentro de su organización y priorizar la retirada de los sistemas operativos heredados de la población de controlador de dominio. Al mantener los controladores de dominio heredados actual y la eliminación de los controladores de dominio, a menudo puede tener las ventajas de la nueva funcionalidad y la seguridad que no estén disponible en los bosques o dominios con controladores de dominio que ejecutan el sistema operativo heredado. Los controladores de dominio deben recién instalados y promueve lugar actualizados desde sistemas operativos anteriores o los roles de servidor; es decir, no realice actualizaciones en contexto de los controladores de dominio ni ejecutar al Asistente para la instalación de AD DS en servidores en el que no esté recién instalado el sistema operativo. Mediante la implementación de controladores de dominio recién instalado, asegúrese de que antiguos archivos y configuraciones no se mantienen involuntariamente en controladores de dominio y simplificar el cumplimiento de configuración del controlador de dominio coherente y segura.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuración segura de controladores de dominio

Un número de las herramientas disponibles gratuitamente, algunos de los cuales se instalan de forma predeterminada en Windows, puede utilizarse para crear una línea de base de configuración de seguridad inicial para los controladores de dominio que posteriormente se puede aplicar a los GPO. Estas herramientas se describen aquí.  
  
### <a name="security-configuration-wizard"></a>Asistente para configuración de seguridad  

Todos los controladores de dominio se deben bloquear tras la compilación inicial. Esto puede lograrse mediante el Asistente para configuración de seguridad que se incluye de forma nativa en Windows Server para configurar el servicio, el registro, sistema y configuración de WFAS en un controlador de dominio de "compilación base". Configuración se puede guardar y exportar a un GPO que se puede vincular a la OU Domain Controllers en cada dominio del bosque para aplicar una configuración coherente de los controladores de dominio. Si el dominio contiene varias versiones de sistemas operativos de Windows, puede configurar filtros de Instrumental de administración de Windows (WMI) para aplicar los GPO únicamente a los controladores de dominio que ejecuten la versión correspondiente del sistema operativo.  
  
### <a name="microsoft-security-compliance-toolkit"></a>Kit de herramientas de compatibilidad de seguridad de Microsoft

[Microsoft Security Compliance Toolkit](https://www.microsoft.com/download/details.aspx?id=55319) configuración del controlador de dominio se puede combinar con la configuración del Asistente para configuración de seguridad para generar líneas de base de configuración completa para los controladores de dominio que se implementan y aplica los GPO implementar en la unidad organizativa controladores de dominio de Active Directory.  
  
### <a name="rdp-restrictions"></a>Restricciones de RDP

Objetos de directiva de grupo que se vinculan a todas las unidades organizativas en un bosque de controladores de dominio debe configurarse para permitir las conexiones RDP solo desde los usuarios autorizados y los sistemas (por ejemplo, servidores de salto). Esto puede lograrse mediante una combinación de configuración de derechos de usuario y la configuración de WFAS y debe implementarse en los GPO para que la directiva se aplica de forma coherente. Si se omite, la siguiente actualización de directiva de grupo devuelve el sistema de la configuración adecuada.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Administración de revisiones y configuración para los controladores de dominio

Aunque pueda parecer contradictorio, considere la posibilidad de aplicar revisiones en los controladores de dominio y otros componentes de infraestructura crítica por separado de la infraestructura general de Windows. Si hace uso de software de administración de configuración de empresa para todos los equipos de su infraestructura, poner en peligro el software de administración de sistemas puede utilizarse para poner en peligro o destruir todos los componentes de infraestructura que ese software administrados. Al separar la administración de revisiones y los sistemas para controladores de dominio de la población general, puede reducir la cantidad de software instalado en los controladores de dominio, además de controlar estrechamente la administración.
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Bloqueo de acceso a Internet para los controladores de dominio  

Una de las comprobaciones que se realiza como parte de una evaluación de seguridad de Active Directory es el uso y la configuración de Internet Explorer en controladores de dominio. Internet Explorer (o cualquier otro explorador web) no debe usarse en controladores de dominio, pero el análisis de miles de controladores de dominio ha revelado numerosos casos en que los usuarios con privilegios usan Internet Explorer para explorar la intranet de la organización o el Internet.  
  
Como se describió anteriormente en la sección "configuración incorrecta" de [vías de compromiso](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), exploración de Internet (o una intranet infectada) desde uno de los equipos más eficaces en una infraestructura de Windows con privilegios elevados cuenta (que son las únicas cuentas que se permite para iniciar sesión localmente en los controladores de dominio de manera predeterminada) presenta un extraordinario riesgo de seguridad de la organización. A través de una unidad de descarga o descarga de infectados por malware "utilidades", los atacantes pueden tener acceso a todo que lo necesario para poner en peligro o destruir el entorno de Active Directory de completamente.  
  
Aunque Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 y las versiones actuales de Internet Explorer ofrecen una serie de protecciones contra descargas malintencionadas, en la mayoría de los casos en el dominio que los controladores y las cuentas con privilegios hubiera usado para navegar por Internet, los controladores de dominio estaban ejecutando Windows Server 2003 o se ha deshabilitado intencionadamente las protecciones que ofrece más recientes de los sistemas operativos y exploradores.  
  
Iniciar exploradores web en los controladores de dominio debe estar prohibido, no solo por la directiva, sino también por los controles técnicos y los controladores de dominio no deberían poder acceder a Internet. Si los controladores de dominio se necesitan replicar entre sitios, debe implementar conexiones seguras entre los sitios. Aunque las instrucciones de configuración detallados están fuera del ámbito de este documento, puede implementar una serie de controles para restringir la capacidad de los controladores de dominio para un mal uso o mal configurado y posteriormente en peligro.  
  
### <a name="perimeter-firewall-restrictions"></a>Restricciones de Firewall perimetral

Los firewalls perimetrales deben configurarse para bloquear conexiones salientes desde controladores de dominio a Internet. Aunque es posible que necesitan controladores de dominio se comuniquen a través de los límites del sitio, se pueden configurar los firewalls perimetrales para permitir la comunicación entre sitios siguiendo las instrucciones proporcionadas en [cómo configurar un firewall para dominios y confianzas](https://support.microsoft.com/kb/179442) en el sitio Web de Microsoft Support.  
  
### <a name="dc-firewall-configurations"></a>Configuraciones de Firewall de DC  

Como se describió anteriormente, debe usar al Asistente para configuración de seguridad para capturar la configuración del Firewall de Windows con seguridad avanzada en controladores de dominio. Debe revisar la salida del Asistente para configuración de seguridad para asegurarse de que la configuración del firewall cumple los requisitos de su organización y, a continuación, usa los GPO para aplicar opciones de configuración.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Impedir la exploración Web desde los controladores de dominio

Puede usar una combinación de configuración de AppLocker, "agujero negro" configuración del proxy y configuración de WFAS para evitar que los controladores de dominio el acceso a Internet y para impedir el uso de los exploradores web en controladores de dominio.
