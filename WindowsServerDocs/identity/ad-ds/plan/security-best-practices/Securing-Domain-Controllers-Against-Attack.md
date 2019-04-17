---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Seguridad de los controladores de dominio contra los ataques
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc40d0ac536b128b799006214e360fa4d991ee44
ms.sourcegitcommit: 06a84f5caeab49b8480d6eed037aebbb59c77a9f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2018
---
# <a name="securing-domain-controllers-against-attack"></a>Seguridad de los controladores de dominio contra los ataques

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Ley número tres: si un intruso tiene acceso físico al equipo, no es el equipo ya.* - [Diez inmutables de seguridad (versión 2.0)](https://technet.microsoft.com/security/hh278941.aspx)  
  
Controladores de dominio proporcionan el almacenamiento físico de la base de datos de AD DS, además de proporcionar los servicios y los datos que permiten a las empresas administrar eficazmente sus servidores, las estaciones de trabajo, los usuarios y aplicaciones. Si se obtiene con privilegios de acceso a un controlador de dominio por un usuario malintencionado, que el usuario puede modificar, dañar o destruir la base de datos de AD DS y, por extensión, todos los sistemas y las cuentas que administran por Active Directory.  
  
Porque los controladores de dominio pueden leer y escribir en cualquier elemento de la base de datos de AD DS, ponga en peligro un controlador de dominio significa que el bosque de Active Directory no puede nunca se consideran de confianza nuevamente a menos que sea capaz de recuperación mediante una copia de seguridad buena y cerrar los huecos que pongan en peligro el permitido en el proceso.  
  
Dependiendo de un atacante modificación de preparación, herramientas y habilidades o un daño irreparable incluso en lo AD DS base de datos puede llevar a cabo en minutos, horas, no días o semanas. Qué no importa cuánto tiempo un atacante ha con privilegios de acceso a Active Directory, pero se obtiene cuánto el atacante ha planeado por el momento al acceso a ellos. Poner en peligro un controlador de dominio puede proporcionar la ruta de acceso más conveniente propagación de gran escala de acceso, o la ruta de acceso más directa la destrucción de Active Directory, servidores miembro y estaciones de trabajo. Por este motivo, los controladores de dominio se deben proteger por separado y más estrictas que la infraestructura general de Windows.  

  
## <a name="physical-security-for-domain-controllers"></a>Seguridad física para controladores de dominio  
Esta sección proporciona información acerca de cómo proteger físicamente los controladores de dominio, si los controladores de dominio son físicos o máquinas virtuales, en ubicaciones de centros de datos, las sucursales y ubicaciones remotas incluso con únicamente los controles de infraestructura básica.  
  
### <a name="datacenter-domain-controllers"></a>Controladores de dominio del centro de datos  
  
#### <a name="physical-domain-controllers"></a>Controladores de dominio físico  
En los centros de datos, controladores de dominio físico deberían estar instalados en bastidores seguros dedicados o cajas que son independientes de la población general del servidor. Cuando sea posible, controladores de dominio deben estar configurados con chips de módulo de plataforma segura (TPM) y todos los volúmenes en los servidores de controlador de dominio deben estar protegidos mediante cifrado de unidad BitLocker. Por lo general, agrega el rendimiento de carga de trabajo en un solo dígito porcentajes BitLocker, pero protege el directorio frente a ataques incluso si los discos se quitan del servidor. BitLocker también puede ayudar a proteger los sistemas frente a ataques, como los rootkits porque la modificación de los archivos de arranque hará que el servidor arrancar en modo de recuperación para que se pueden cargar los archivos binarios originales. Si un controlador de dominio está configurado para usar el software RAID, SCSI conectados en serie, almacenamiento de SAN/NAS, o no se puede implementar volúmenes dinámicos, BitLocker, por lo tanto, dispositivos de almacenamiento locales (con o sin RAID de hardware) deben usarse en los controladores de dominio siempre que sea posible.  
  
#### <a name="virtual-domain-controllers"></a>Controladores de dominio virtual  
Si implementas los controladores de dominio virtual, debes asegurarte de que los controladores de dominio se ejecutarán en hosts físicos independientes que otras máquinas virtuales en el entorno. Incluso si usas una plataforma de virtualización de terceros, considera la posibilidad de implementar los controladores de dominio virtual en Hyper-V Server en Windows Server 2012 o Windows Server 2008 R2, que proporciona una superficie de ataque mínima y puede administrarse con los controladores de dominio que hospeda en lugar de que se administran con el resto de los hosts de virtualización. Si implementas System Center Virtual Machine Manager (SCVMM) para la administración de la infraestructura de virtualización, puede delegar la administración de los hosts físicos en la residen las máquinas virtuales de controlador de dominio y los propios controladores de dominio a los administradores autorizados. También debe considerar separar el almacenamiento de controladores de dominio virtual para evitar que los administradores de almacenamiento de acceso a los archivos de la máquina virtual.  
  
### <a name="branch-locations"></a>Sucursales  
  
#### <a name="physical-domain-controllers"></a>Controladores de dominio físico  
En ubicaciones en el que varios servidores residen pero no están asegurados físicamente en la medida que está protegidos datacenter Server, controladores de dominio físico deben estar configurados con chips de TPM y el cifrado de unidad BitLocker para todos los volúmenes del servidor. Si un controlador de dominio no pueden almacenarse en una sala bloqueada en sucursales, considera la posibilidad de implementar RODC de esas ubicaciones.  
  
#### <a name="virtual-domain-controllers"></a>Controladores de dominio virtual  
Siempre que sea posible, debes ejecutar controladores de dominio virtual en sucursales en hosts físicos independientes que las otras máquinas virtuales en el sitio. En las sucursales en el que los controladores de dominio virtual no se pueden ejecutar en hosts físicos independientes del resto de la población de servidor virtual, debe implementar chips de TPM y cifrado de unidad BitLocker en los hosts en el que los controladores de dominio virtual se ejecuten como mínimo y todos los hosts si es posible. Según el tamaño de las sucursales y la seguridad de los hosts físicos, considera la posibilidad de implementar RODC en sucursales.  
  
### <a name="remote-locations-with-limited-space-and-security"></a>Ubicaciones remotas con espacio limitado y seguridad  
Si su infraestructura incluye ubicaciones en el que se puede instalar un único servidor físico, un servidor capaz de ejecutar cargas de trabajo de virtualización debe instalarse en la ubicación remota y cifrado de unidad BitLocker deben estar configurado para proteger todos los volúmenes en el servidor. Una máquina virtual en el servidor debe ejecutarse un RODC con otros servidores que ejecuten como máquinas virtuales independientes en el host. Información sobre cómo planificar la implementación de RODC se proporciona en el [Guía de implementación y planificación de controlador de dominio de solo lectura](https://go.microsoft.com/fwlink/?LinkID=135993). Para obtener más información sobre la implementación y seguridad de los controladores de dominio virtualizado, consulta [ejecutando los controladores de dominio en Hyper-V](https://technet.microsoft.com/library/dd363553(v=ws.10).aspx) en el sitio Web de TechNet. Para obtener más instrucciones para reforzar Hyper-V, delegar la administración de máquinas virtuales y proteger máquinas virtuales, consulta el [Guía de seguridad de Hyper-V](https://www.microsoft.com/download/details.aspx?id=16650) Acelerador de soluciones en el sitio Web de Microsoft.  
  
## <a name="domain-controller-operating-systems"></a>Sistemas operativos de controlador de dominio  
Debes ejecutar todos los controladores de dominio en la versión más reciente de Windows Server que se admite dentro de la organización y dar prioridad a la retirada de los sistemas operativos heredados de la población de controlador de dominio. Al mantener los controladores de dominio heredado actual y la eliminación de los controladores de dominio, a menudo se puede sacar provecho de la nueva funcionalidad y la seguridad que no estén disponible en los dominios o bosques con controladores de dominio que ejecutan sistemas operativos heredados. Controladores de dominio deben ser recién instalados y promueve en lugar de actualizado desde sistemas operativos anteriores o roles de servidor; es decir, no realizar actualizaciones en contexto de controladores de dominio ni ejecutar al Asistente para la instalación de AD DS en servidores en los que no recién instalado el sistema operativo. Al implementar controladores de dominio recién instalados, asegúrate de que configuración y los archivos antiguos no accidentalmente quedan en controladores de dominio y simplificar la aplicación de configuración del controlador de dominio, coherente y segura.  
  
## <a name="secure-configuration-of-domain-controllers"></a>Configuración de controladores de dominio segura  
Un número de herramientas disponibles gratuitas, algunos de los cuales se instalan de forma predeterminada en Windows, puede usarse para crear una referencia de configuración de seguridad inicial para controladores de dominio que pueden aplicarse posteriormente a los GPO. Estas herramientas se describen aquí.  
  
### <a name="security-configuration-wizard"></a>Asistente para configuración de seguridad  

Todos los controladores de dominio deben bloquearse tras la compilación inicial. Esto se consigue mediante el Asistente para configuración de seguridad que se incluye de forma nativa en Windows Server para configurar el servicio, el registro, el sistema y configuración WFAS en un controlador de dominio "compilación base". Opciones de configuración se pueda guardar y exportadas a un GPO que se puede vincular a controladores de dominio en cada dominio en el bosque para aplicar una configuración coherente de controladores de dominio. Si tu dominio contiene varias versiones de sistemas operativos Windows, puedes configurar los filtros de Instrumental de administración de Windows (WMI) para los GPO se aplican solo a los controladores de dominio que ejecutan la versión del sistema operativo correspondiente.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
[Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) configuración del controlador de dominio puede combinarse con la configuración del Asistente para configuración de seguridad para producir líneas de base de configuración completo para controladores de dominio que se han implementado y aplican a los GPO implementados en la unidad organizativa de controladores de dominio de Active Directory.  
  
### <a name="applocker"></a>AppLocker  
AppLocker o una herramienta de la lista blanca de aplicaciones de terceros debe usarse para configurar los servicios y aplicaciones que tienen permiso para ejecutarse en controladores de dominio, y estos servicios y aplicaciones permitidas deben estar formados por solo lo que se necesita para que el equipo host de AD DS y posiblemente DNS, además de cualquier software de seguridad del sistema como software antivirus. Lista blanca permitido aplicaciones en controladores de dominio, se agrega una capa de seguridad adicional para que incluso si una aplicación no autorizada está instalada en un controlador de dominio, no se puede ejecutar la aplicación.  
  
### <a name="rdp-restrictions"></a>Restricciones de RDP  
Los objetos de directiva de grupo vinculados a todas las unidades organizativas en un bosque de controladores de dominio deben estar configurados para permitir las conexiones RDP Emitirán solo de los usuarios autorizados y sistemas (por ejemplo, los servidores de accesos directos). Esto puede lograrse mediante una combinación de opciones de derechos de usuario y la configuración de WFAS y debe implementarse en los GPO para que la directiva se aplica de forma coherente. Si lo se omite, la próxima actualización de directiva de grupo devuelve el sistema a su configuración sea correcta.  
  
### <a name="patch-and-configuration-management-for-domain-controllers"></a>Revisión y administración de la configuración de controladores de dominio  
Aunque puede parecer poco intuitiva, considera la posibilidad de revisar los controladores de dominio y otros componentes de infraestructura crítica por separado de la infraestructura general de Windows. Si aprovecha el software de administración de configuración de empresa para todos los equipos en tu infraestructura, ponga en peligro el software de administración de sistemas puede usarse para poner en peligro o destruir todos los componentes de infraestructura que administrados por ese software. Separando revisión y sistemas de administración de controladores de dominio de la población en general, puede reducir la cantidad de software instalado en controladores de dominio, además de controlar estrechamente su administración.  
  
### <a name="blocking-internet-access-for-domain-controllers"></a>Bloquear el acceso a Internet para controladores de dominio  

Uno de los controles que se realiza como parte de una evaluación de seguridad de Active Directory es el uso y la configuración de Internet Explorer en controladores de dominio. Internet Explorer (o cualquier otro explorador web) no debe usarse en los controladores de dominio, pero el análisis de miles de controladores de dominio se puso de manifiesto numerosos casos en que los usuarios con privilegios usan Internet Explorer para navegar por Internet o intranet de la organización.  
  
Como se describió anteriormente en la sección "Configuración" de [vías peligro](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), exploración de Internet (o en una intranet infectada) de uno de los equipos más eficaces en una infraestructura de Windows con una cuenta con privilegios elevados (que son las únicas cuentas puedas iniciar sesión localmente en controladores de dominio predeterminada) presenta un riesgo extraordinario a la seguridad de la organización. A través de una unidad de descarga o descarga de infección de malware "utilidades", los atacantes pueden acceder a todo que lo necesario para poner en peligro completamente o destruir el entorno de Active Directory.  
  
Aunque Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 y las versiones actuales de Internet Explorer ofrecen una serie de protección contra descargas malintencionadas, en la mayoría de los casos en qué dominio controladores y las cuentas con privilegios habían utilizadas para navegar por Internet, los controladores de dominio ejecutan Windows Server 2003 o se ha deshabilitado intencionadamente protecciones que ofrece más reciente de los sistemas operativos y exploradores.  
  
Iniciar los exploradores web en los controladores de dominio debes prohibir no solo por la directiva, pero no por controles técnicos y los controladores de dominio no deberían poder acceder a Internet. Si los controladores de dominio necesitan replicar en todos los sitios, debes implementar conexiones seguras entre los sitios. Aunque las instrucciones de configuración detallada están fuera del ámbito de este documento, puedes implementar una serie de controles para restringir la capacidad de controladores de dominio para el uso incorrecto o incorrecta y posteriormente en peligro.  
  
### <a name="perimeter-firewall-restrictions"></a>Restricciones de Firewall del perímetro  
Los firewalls perimetrales deben configurarse para bloquear las conexiones de controladores de dominio salientes a Internet. Aunque los controladores de dominio pueden necesitar comunicarse a través de los límites del sitio, firewalls perimetrales pueden configurarse para permitir la comunicación entre sitios siguiendo las directrices incluidas en [cómo configurar un firewall para dominios y confianzas](https://support.microsoft.com/kb/179442) en el sitio Web de Microsoft Support.  
  
### <a name="dc-firewall-configurations"></a>Configuraciones de Firewall de DC  

Como se describió anteriormente, debes usar al Asistente para configuración de seguridad para capturar la configuración de Firewall de Windows con seguridad avanzada en controladores de dominio. Debes revisar la salida del Asistente para configuración de seguridad para garantizar que las opciones de configuración de firewall cumplan los requisitos de la organización y, a continuación, usan los GPO para aplicar las opciones de configuración.  
  
### <a name="preventing-web-browsing-from-domain-controllers"></a>Evitar la exploración Web de controladores de dominio  
Puedes usar una combinación de configuración de AppLocker, la configuración de proxy "agujero negro" y la configuración de WFAS para impedir que los controladores de dominio tenga acceso a Internet e impedir el uso de los exploradores web en los controladores de dominio.  
  


