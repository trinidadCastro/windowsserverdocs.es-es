---
ms.assetid: ba28bd05-16e6-465f-982b-df49633cfde4
title: Protección de los controladores de dominio frente a ataques
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 06/18/2017
ms.topic: article
ms.openlocfilehash: 0ac2a3b67b0c3407db017c63d5e5187d36f08aa5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958853"
---
# <a name="securing-domain-controllers-against-attack"></a>Protección de los controladores de dominio frente a ataques

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Número de ley tres: Si un intruso tiene acceso físico sin restricciones al equipo, ya no es el equipo.* - [Diez leyes inmutables de seguridad (versión 2,0)](https://www.microsoft.com/en-us/msrc?rtc=1)

Los controladores de dominio proporcionan el almacenamiento físico para la base de datos AD DS, además de proporcionar los servicios y los datos que permiten a las empresas administrar de forma eficaz sus servidores, estaciones de trabajo, usuarios y aplicaciones. Si un usuario malintencionado obtiene acceso con privilegios a un controlador de dominio, dicho usuario puede modificar, dañar o destruir la base de datos de AD DS y, por extensión, todos los sistemas y cuentas administrados por Active Directory.

Dado que los controladores de dominio pueden leer y escribir en cualquier parte de la base de datos de AD DS, poner en peligro un controlador de dominio significa que el bosque de Active Directory nunca se puede considerar de confianza de nuevo a menos que pueda realizar la recuperación mediante una copia de seguridad correcta conocida y cerrar los huecos que permitían el riesgo en el proceso.

En función de la preparación, las herramientas y los conocimientos, la modificación o incluso los daños irreparables en el AD DS base de datos, se pueden completar en minutos hasta horas, no en días o semanas. Lo que importa no es el tiempo durante el que un atacante tiene privilegios de acceso a Active Directory, pero cuánto ha planeado el atacante en el momento en que se obtiene el acceso con privilegios. Poner en peligro un controlador de dominio puede proporcionar la ruta de acceso más expedita a la propagación de acceso a gran escala o la ruta de acceso más directa a la destrucción de servidores miembro, estaciones de trabajo y Active Directory. Por este motivo, los controladores de dominio se deben proteger por separado y más rigurosamente que la infraestructura de Windows general.

## <a name="physical-security-for-domain-controllers"></a>Seguridad física de los controladores de dominio

En esta sección se proporciona información acerca de cómo proteger físicamente los controladores de dominio, si los controladores de dominio son máquinas físicas o virtuales, en ubicaciones de centros de datos, en sucursales e incluso en ubicaciones remotas con controles de infraestructura básica únicamente.

### <a name="datacenter-domain-controllers"></a>Controladores de dominio de Datacenter

#### <a name="physical-domain-controllers"></a>Controladores de dominio físicos

En los centros de recursos, los controladores de dominio físicos deben instalarse en bastidores seguros dedicados o en jaulas independientes del rellenado de servidor general. Cuando sea posible, los controladores de dominio deben configurarse con chips Módulo de plataforma segura (TPM) y todos los volúmenes de los servidores de controlador de dominio deben protegerse mediante Cifrado de unidad BitLocker. Normalmente, BitLocker agrega una sobrecarga de rendimiento en porcentajes de un solo dígito, pero protege el directorio contra el riesgo incluso si se quitan los discos del servidor. BitLocker también puede ayudar a proteger los sistemas frente a ataques como rootkits, ya que la modificación de los archivos de arranque hará que el servidor arranque en modo de recuperación para que se puedan cargar los archivos binarios originales. Si un controlador de dominio está configurado para usar RAID de software, SCSI conectado en serie, almacenamiento SAN/NAS o volúmenes dinámicos, BitLocker no se puede implementar, por lo que el almacenamiento conectado localmente (con o sin RAID de hardware) debe usarse en los controladores de dominio siempre que sea posible.

#### <a name="virtual-domain-controllers"></a>Controladores de dominio virtuales

Si implementa controladores de dominio virtuales, debe asegurarse de que los controladores de dominio se ejecutan en hosts físicos independientes que en otras máquinas virtuales del entorno. Incluso si usa una plataforma de virtualización de terceros, considere la posibilidad de implementar controladores de dominio virtuales en el servidor de Hyper-V en Windows Server 2012 o Windows Server 2008 R2, lo que proporciona una superficie de ataque mínima y se puede administrar con los controladores de dominio que hospeda en lugar de administrarlos con el resto de los hosts de virtualización. Si implementa System Center Virtual Machine Manager (SCVMM) para la administración de la infraestructura de virtualización, puede delegar la administración de los hosts físicos en los que residen las máquinas virtuales del controlador de dominio y los propios controladores de dominio para los administradores autorizados. También debe considerar la posibilidad de separar el almacenamiento de controladores de dominio virtuales para evitar que los administradores de almacenamiento tengan acceso a los archivos de la máquina virtual.

### <a name="branch-locations"></a>Ubicaciones de rama

#### <a name="physical-domain-controllers-in-branches"></a>Controladores de dominio físicos en ramas

En ubicaciones en las que residen varios servidores, pero que no están protegidos físicamente hasta el grado de protección de los servidores de centros de seguridad, los controladores de dominio físicos deben configurarse con chips de TPM y Cifrado de unidad BitLocker para todos los volúmenes del servidor. Si un controlador de dominio no se puede almacenar en una sala bloqueada en ubicaciones de sucursales, considere la posibilidad de implementar RODC en esas ubicaciones.

#### <a name="virtual-domain-controllers-in-branches"></a>Controladores de dominio virtuales en ramas

Siempre que sea posible, debe ejecutar los controladores de dominio virtuales de las sucursales en hosts físicos independientes que las otras máquinas virtuales del sitio. En las sucursales en las que los controladores de dominio virtuales no se pueden ejecutar en hosts físicos independientes del resto del rellenado de Virtual Server, debe implementar los chips de TPM y Cifrado de unidad BitLocker en los hosts en los que los controladores de dominio virtuales se ejecuten como mínimo, y todos los hosts si es posible. En función del tamaño de la sucursal y de la seguridad de los hosts físicos, debe considerar la posibilidad de implementar RODC en ubicaciones de sucursales.

### <a name="remote-locations-with-limited-space-and-security"></a>Ubicaciones remotas con espacio limitado y seguridad

Si la infraestructura incluye ubicaciones en las que solo se puede instalar un único servidor físico, se debe instalar un servidor capaz de ejecutar cargas de trabajo de virtualización en la ubicación remota y Cifrado de unidad BitLocker debe configurarse para proteger todos los volúmenes del servidor. Una máquina virtual en el servidor debe ejecutar un RODC, con otros servidores que se ejecuten como máquinas virtuales independientes en el host. La información sobre la planeación de la implementación de RODC se proporciona en la [Guía de planeamiento e implementación del controlador de dominio de solo lectura](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771744(v=ws.10)). Para obtener más información acerca de la implementación y la protección de controladores de dominio virtualizados, consulte [controladores de dominio en ejecución en Hyper-V](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd363553(v=ws.10)). Para obtener instrucciones más detalladas para la protección de Hyper-V, la delegación de la administración de máquinas virtuales y la protección de máquinas virtuales, vea el acelerador de la solución de la [Guía de seguridad de Hyper-v](https://www.microsoft.com/download/details.aspx?id=16650) en el sitio web de Microsoft.

## <a name="domain-controller-operating-systems"></a>Sistemas operativos de controlador de dominio

Debe ejecutar todos los controladores de dominio en la versión más reciente de Windows Server que sea compatible con su organización y dar prioridad a la retirada de los sistemas operativos heredados en el rellenado del controlador de dominio. Al mantener actualizados los controladores de dominio y eliminar los controladores de dominio heredados, a menudo puede aprovechar la nueva funcionalidad y la seguridad que puede que no estén disponibles en dominios o bosques con controladores de dominio que ejecuten el sistema operativo heredado. Los controladores de dominio se deben instalar y promocionar de nuevo en lugar de actualizarse a partir de los sistemas operativos o roles de servidor anteriores. es decir, no realice actualizaciones en contexto de controladores de dominio ni ejecute el Asistente para la instalación de AD DS en servidores en los que el sistema operativo no esté instalado de nuevo. Mediante la implementación de controladores de dominio recién instalados, se asegura de que los archivos y la configuración heredados no queden involuntariamente en los controladores de dominio y se simplifique la aplicación de una configuración de controlador de dominio segura y coherente.

## <a name="secure-configuration-of-domain-controllers"></a>Configuración segura de controladores de dominio

Una serie de herramientas disponibles libremente, algunas de las cuales están instaladas de forma predeterminada en Windows, se pueden usar para crear una línea de base de configuración de seguridad inicial para los controladores de dominio que se pueden aplicar posteriormente a los GPO. Estas herramientas se describen aquí.

### <a name="security-configuration-wizard"></a>Asistente para configuración de seguridad

Todos los controladores de dominio deben estar bloqueados tras la compilación inicial. Esto puede lograrse mediante el Asistente para configuración de seguridad que se distribuye de forma nativa en Windows Server para configurar el servicio, el registro, el sistema y la configuración de WFAS en un controlador de dominio de "compilación base". La configuración se puede guardar y exportar a un GPO que se puede vincular a la unidad organizativa controladores de dominio en cada dominio del bosque para aplicar una configuración coherente de los controladores de dominio. Si el dominio contiene varias versiones de los sistemas operativos Windows, puede configurar filtros de Instrumental de administración de Windows (WMI) para aplicar los GPO únicamente a los controladores de dominio que ejecutan la versión correspondiente del sistema operativo.

### <a name="microsoft-security-compliance-toolkit"></a>Kit de herramientas de cumplimiento de seguridad de Microsoft

La configuración del controlador de dominio del [Kit de herramientas de cumplimiento de seguridad de Microsoft](https://microsoft.com/download/details.aspx?id=55319) puede combinarse con la configuración del Asistente para configuración de seguridad para generar líneas de base de configuración completas para los controladores de dominio implementados y aplicados por los GPO implementados en la unidad organizativa controladores de dominio en Active Directory.

### <a name="rdp-restrictions"></a>Restricciones de RDP

Directiva de grupo objetos que se vinculan a todas las unidades organizativas de controladores de dominio de un bosque deben configurarse para permitir conexiones RDP solo de usuarios y sistemas autorizados (por ejemplo, servidores de salto). Esto se puede lograr a través de una combinación de la configuración de los derechos de usuario y la configuración del WFAS y debe implementarse en los GPO para que la Directiva se aplique de forma coherente. Si se omite, la siguiente actualización de directiva de grupo devuelve el sistema a su configuración adecuada.

### <a name="patch-and-configuration-management-for-domain-controllers"></a>Administración de revisiones y configuración para controladores de dominio

Aunque puede parecer un poco intuitivo, debe considerar la posibilidad de aplicar revisiones a los controladores de dominio y otros componentes de infraestructura críticos de forma independiente de la infraestructura de Windows general. Si aprovecha el software de administración de configuración empresarial para todos los equipos de la infraestructura, se puede usar el riesgo del software de administración de sistemas para poner en peligro o destruir todos los componentes de la infraestructura administrados por ese software. Al separar la revisión y la administración de sistemas para los controladores de dominio de la población general, puede reducir la cantidad de software instalado en los controladores de dominio, además de controlar de forma estricta su administración.

### <a name="blocking-internet-access-for-domain-controllers"></a>Bloquear el acceso a Internet para controladores de dominio

Una de las comprobaciones que se realizan como parte de un Active Directory evaluación de seguridad es el uso y la configuración de Internet Explorer en los controladores de dominio. Internet Explorer (o cualquier otro explorador Web) no se debe usar en los controladores de dominio, pero el análisis de miles de controladores de dominio ha revelado numerosos casos en los que los usuarios con privilegios usaban Internet Explorer para explorar la intranet o Internet de la organización.

Tal como se ha descrito anteriormente en la sección "configuración inestable" de [caminos para poner en peligro](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md), explorar Internet (o una intranet infectada) desde uno de los equipos más potentes en una infraestructura de Windows con una cuenta con privilegios elevados (que son las únicas cuentas con las que se permite el inicio de sesión local en los controladores de dominio de forma predeterminada) supone un riesgo extraordinario Ya sea a través de una unidad mediante la descarga o la descarga de utilidades infectadas por malware, los atacantes pueden obtener acceso a todo lo que necesitan para poner en peligro o destruir el Active Directory entorno.

Aunque Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 y las versiones actuales de Internet Explorer ofrecen una serie de protecciones contra descargas malintencionadas, en la mayoría de los casos en los que los controladores de dominio y las cuentas con privilegios se usaban para explorar Internet, los controladores de dominio ejecutaban Windows Server 2003 o las protecciones ofrecidas por sistemas operativos y exploradores más recientes

El inicio de los exploradores Web en los controladores de dominio debe estar prohibido, no solo por la Directiva, sino también por los controles técnicos, y los controladores de dominio no deben tener permiso de acceso a Internet. Si los controladores de dominio necesitan replicarse entre sitios, debe implementar conexiones seguras entre los sitios. Aunque las instrucciones de configuración detalladas están fuera del ámbito de este documento, puede implementar una serie de controles para restringir la capacidad de los controladores de dominio de ser usados incorrectamente o mal configurados y en peligro.

### <a name="perimeter-firewall-restrictions"></a>Restricciones del firewall perimetral

Los firewalls perimetrales deben configurarse para bloquear las conexiones salientes desde controladores de dominio a Internet. Aunque los controladores de dominio pueden necesitar comunicarse a través de los límites del sitio, los firewalls perimetrales se pueden configurar para permitir la comunicación entre sitios siguiendo las instrucciones [que se proporcionan en How to Configure a Firewall for Active Directory Domains and trusts](https://support.microsoft.com/kb/179442) en el sitio web de soporte técnico de Microsoft.

### <a name="dc-firewall-configurations"></a>Configuraciones de Firewall de DC

Como se describió anteriormente, debe usar el Asistente para configuración de seguridad para capturar la configuración de Firewall de Windows con seguridad avanzada en los controladores de dominio. Debe revisar el resultado del Asistente para configuración de seguridad para asegurarse de que las opciones de configuración del firewall cumplan los requisitos de su organización y, a continuación, utilizar los GPO para aplicar las opciones de configuración.

### <a name="preventing-web-browsing-from-domain-controllers"></a>Impedir la exploración Web desde controladores de dominio

Puede usar una combinación de configuración de AppLocker, configuración de proxy de "agujero negro" y configuración de WFAS para impedir que los controladores de dominio tengan acceso a Internet y evitar el uso de exploradores Web en los controladores de dominio.
