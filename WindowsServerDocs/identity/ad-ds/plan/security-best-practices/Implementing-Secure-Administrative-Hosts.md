---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementación de hosts administrativos seguros
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ed2ff7bfa0cc3b27506b1ca324e819860eef314c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890426"
---
# <a name="implementing-secure-administrative-hosts"></a>Implementación de hosts administrativos seguros

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Hosts administrativos seguros son las estaciones de trabajo o servidores que se han configurado específicamente para los fines de creación de plataformas seguras desde el que las cuentas con privilegios pueden realizar tareas administrativas en Active Directory o en controladores de dominio los sistemas unidos a un dominio y aplicaciones que se ejecutan en sistemas unidos a un dominio. En este caso, "cuentas con privilegios" hace referencia no solo a las cuentas que son miembros de los grupos con más privilegios en Active Directory, sino también a las cuentas que se han delegado los derechos y permisos que permiten realizar las tareas administrativas.  
  
Estas cuentas pueden ser cuentas de soporte técnico que tienen la capacidad de restablecer contraseñas para la mayoría de los usuarios en un dominio, las cuentas que se usan para administrar registros y zonas DNS, o cuentas que se usan para la administración de configuración. Hosts administrativos seguros están dedicados a la funcionalidad administrativa y que no se ejecuten software como aplicaciones de correo electrónico, exploradores web o software de productividad como Microsoft Office.  
  
Aunque las cuentas y grupos "con más privilegios" en consecuencia deben ser la forma más rigurosa protegido, esto no elimina la necesidad de proteger las cuentas y grupos a qué privilegios superiores a los de usuario estándar tienen cuentas.  
  
Puede ser una estación de trabajo dedicado que se utiliza sólo para tareas administrativas, un servidor miembro que ejecuta el rol de servidor de puerta de enlace de escritorio remoto y a qué usuarios de TI conectarse para llevar a cabo la administración de hosts de destino o un servidor que ejecuta un host administrativo seguro el rol de Hyper-V y proporciona una única máquina virtual para cada usuario que se usará para sus tareas administrativas de TI. En muchos entornos, se pueden implementar combinaciones de los tres enfoques.  
  
Implementación de hosts administrativos seguros requiere planeación y configuración que es coherente con el tamaño de su organización, las prácticas administrativas, apetito de riesgo y el presupuesto. Consideraciones y opciones para implementar hosts administrativos seguros se muestran aquí para que pueda usar para desarrollar una estrategia administrativa adecuada para su organización.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principios para la creación de Hosts administrativos seguros  
Para proteger los sistemas frente a ataques de forma eficaz, deben tenerse en cuenta algunos principios generales:  
  
1.  Nunca debe administrar un sistema de confianza (es decir, un servidor seguro como un controlador de dominio) desde un host de menor confianza (es decir, una estación de trabajo que no está protegido en la misma medida que los sistemas que administra).  
  
2.  No debe confiar en un único factor de autenticación al realizar actividades con privilegios. es decir, las combinaciones de nombre y la contraseña de usuario no deben considerarse autenticación aceptable porque se representa un solo factor (algo que sabe). Debe considerar donde credenciales se genera y almacena en caché o se almacenan en escenarios administrativos.  
  
3.  Aunque la mayoría de los ataques en el panorama de amenazas actual aprovechar malware y piratería malintencionados, no omita la seguridad física al diseñar e implementar los hosts administrativos seguros.  
  
### <a name="account-configuration"></a>Configuración de la cuenta  
Si su organización no usa actualmente las tarjetas inteligentes, considere la posibilidad de implementarlos para las cuentas con privilegios y hosts administrativos seguros. Hosts administrativos deben configurarse para requerir inicio de sesión de tarjeta inteligente para todas las cuentas mediante la modificación de la siguiente configuración en un GPO que esté vinculado a la OU que contiene hosts administrativos:  
  
**Inicio de sesión de equipo equipo\Directivas\Configuración seguridad\Directivas locales\Opciones seguridad\Inicio: Necesita una tarjeta inteligente**  
  
Esta configuración requiere que todos los inicios de sesión interactivos para usar una tarjeta inteligente, independientemente de la configuración en una cuenta individual en Active Directory.  
  
También debe configurar los hosts administrativos seguros para permitir los inicios de sesión solo por las cuentas autorizadas, que se pueden configurar en:  
  
**Equipo equipo\Directivas\Configuración seguridad\Directivas locales\Opciones configuración de seguridad\Directivas Locales\asignación de derechos**  
  
Esto concede interactivo (y, si procede, servicios de escritorio remoto) derechos de inicio de sesión solo a los usuarios autorizados del host administrativo seguro.  
  
### <a name="physical-security"></a>Seguridad física  
Los hosts administrativos se consideren de confianza, deben configurarse y protegidas en la misma medida que los sistemas que administran. La mayoría de las recomendaciones proporcionadas en [protección de controladores de dominio frente a ataque](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) también son aplicables a los hosts que se usan para administrar controladores de dominio y la base de datos de AD DS. Uno de los desafíos de la implementación de sistemas seguros de administrativos en la mayoría de los entornos es que la seguridad física puede ser más difícil de implementar, ya que estos equipos suelen residan en áreas que no son tan seguras como servidores hospedados en centros de datos, como escritorios de los usuarios administrativos.  
  
Seguridad física incluye controlar el acceso físico a hosts administrativos. En una organización pequeña, esto puede significar que mantienen una estación de trabajo administrativa dedicada que se mantiene bloqueada en una oficina o un cajón cuando no esté en uso. O bien, puede significar que cuando necesite realizar la administración de Active Directory o los controladores de dominio, iniciar sesión en el controlador de dominio directamente.  
  
En organizaciones medianas, puede considerar la implementación segura administrativo "servidores de salto" que se encuentran en una ubicación segura en una oficina y se usan cuando se requiere la administración de Active Directory o controladores de dominio. También puede implementar estaciones de trabajo administrativas que están bloqueados en ubicaciones seguras cuando no esté en uso, con o sin servidores de salto.  
  
En organizaciones grandes, puede implementar servidores de salto alojada en centros de datos que proporcionan un acceso estrictamente controlado a Active Directory; controladores de dominio; y servidores de archivos, impresión o aplicación. Implementación de una arquitectura de servidor de salto es probablemente debe incluir una combinación de servidores y estaciones de trabajo seguras en entornos de gran tamaño.  
  
Independientemente del tamaño de su organización y el diseño de los hosts administrativos, debe proteger los equipos físicos frente a accesos no autorizados o el robo y debe usar cifrado de unidad BitLocker para cifrar y proteger las unidades en hosts administrativos . Mediante la implementación de BitLocker en hosts administrativos, incluso si se roba un host o se quitan sus discos, puede asegurarse de que los datos de la unidad están inaccesibles a los usuarios no autorizados.  
  
### <a name="operating-system-versions-and-configuration"></a>Configuración y las versiones de sistema operativo  
Todos los hosts administrativos, si los servidores o estaciones de trabajo, debe ejecutar el sistema operativo más reciente en el uso de la organización por las razones descritas anteriormente en este documento. Mediante la ejecución de sistemas operativos actuales, el personal administrativo se beneficia de nuevas características de seguridad completa proveedor y la compatibilidad con funcionalidad adicional que se introdujo en el sistema operativo. Además, cuando un nuevo sistema operativo, se evalúa mediante la implementación de hosts de la primera para administrativas, debe familiarizarse con las nuevas características, configuración y los mecanismos de administración que ofrece, que posteriormente se pueden utilizar en la planeación implementación más amplia del sistema operativo. Entonces, los usuarios más sofisticados en su organización también será a los usuarios que están familiarizados con el nuevo sistema operativo y en mejor posición para admitirla.  
  
### <a name="microsoft-security-configuration-wizard"></a>Asistente para configuración de seguridad de Microsoft  
Si implementa servidores de salto como parte de su estrategia de administración de host, debe usar al Asistente para configuración de seguridad integrados para configurar el servicio, el registro, auditoría y configuración del firewall para reducir la superficie expuesta a ataques del servidor. Cuando se recopilan y configuradas las opciones de configuración del Asistente para configuración de seguridad, la configuración se puede convertir a un GPO que se utiliza para exigir una configuración de referencia coherente en todos los servidores de salto. Puede seguir editando el GPO para implementar configuraciones de seguridad específicas para servidores de salto y puede combinar todas las configuraciones con la configuración de línea base adicionales extraídos del Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Administrador de cumplimiento de seguridad de Microsoft  
El [Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) es una herramienta gratuita que integra las configuraciones de seguridad recomendadas por Microsoft, según la configuración de versión y el rol de sistema operativo y los recopila en un herramienta única y la interfaz de usuario que se puede usar para crear y configurar la configuración de seguridad de línea de base para los controladores de dominio. Plantillas de Microsoft Security Compliance Manager se pueden combinar con la configuración del Asistente para configuración de seguridad para generar líneas de base de configuración completa para servidores de salto que se implementan y aplica los GPO implementados en las unidades organizativas en qué salto son servidores Si se encuentra en Active Directory.  
  
> [!NOTE]  
> Cuando se redactó este documento, Microsoft Security Compliance Manager no incluye la configuración específica para servidores de salto o de otros hosts administrativos seguros, pero Security Compliance Manager (SCM) aún puede usarse para crear líneas de base iniciales para administración hosts. Sin embargo, para proteger correctamente los hosts, debe aplicar configuración de seguridad adicionales a los servidores y estaciones de trabajo de alta seguridad.  
  
### <a name="applocker"></a>AppLocker  
Hosts administrativos y machinesshould virtual configurarse con la secuencia de comandos, herramienta y listas blancas de aplicaciones a través de AppLocker o una aplicación de terceros restricción de software. Las aplicaciones administrativas o utilidades que no se cumplen para configuraciones de seguridad deben actualizarse o reemplazarse con las herramientas que se adhiere a las prácticas administrativas y de desarrollo seguro. Cuando se necesitan herramientas nuevas o adicionales en un host administrativo, las utilidades y aplicaciones se deben probar exhaustivamente y si las herramientas son adecuada para su implementación en los hosts administrativos, pueden agregarse a listas blancas de los sistemas.  
  
### <a name="rdp-restrictions"></a>Restricciones de RDP  
Aunque la configuración específica variará según la arquitectura de los sistemas de administración, debe incluir las restricciones en el que se pueden usar las cuentas y los equipos para establecer conexiones de protocolo de escritorio remoto (RDP) a sistemas administrados, servidores para controlar el acceso a controladores de dominio y otros sistemas administrados de los usuarios autorizados y sistemas de salto como el uso de la puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto).  
  
Debe permitir inicios de sesión interactivo de los usuarios autorizados y debe quitar o incluso bloquear otros tipos de inicio de sesión que no son necesarios para el acceso al servidor.  
  
### <a name="patch-and-configuration-management"></a>Administración de revisiones y configuración  
Las organizaciones más pequeñas pueden depender de ofertas como Windows Update o [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) para administrar la implementación de actualizaciones en sistemas Windows, mientras que las organizaciones más grandes pueden implementar la revisión de la empresa y software de administración de configuración, como System Center Configuration Manager. Independientemente de los mecanismos de que usar para implementar actualizaciones en el servidor general y el rellenado de la estación de trabajo, debe considerar implementaciones independientes para los sistemas altamente seguros como controladores de dominio, las entidades de certificación y hosts administrativos. Mediante la separación de estos sistemas de la infraestructura de administración general, si las cuentas de software o servicio de administración están en peligro, peligro no puede fácilmente ampliarse a los sistemas en su infraestructura más seguros.  
  
Aunque no se deben implementar procesos de actualización manual para los sistemas seguros, debe configurar una infraestructura independiente para la actualización de sistemas seguros. Incluso en organizaciones muy grandes, esta infraestructura puede implementarse normalmente a través de servidores WSUS dedicados y GPO para los sistemas protegidos.  
  
### <a name="blocking-internet-access"></a>Bloquear el acceso a Internet  
Hosts administrativos no deberían poder acceder a Internet, ni se debe poder examinar la intranet de la organización. No se deben permitir los exploradores Web y aplicaciones similares en hosts administrativos. Puede bloquear el acceso a Internet para los hosts seguros a través de una combinación de configuración del firewall perimetral, configuración de WFAS y configuración del proxy "agujero negro" hosts seguros. También puede usar listas de aplicaciones permitidas para impedir que los exploradores web que se va a usar en los hosts administrativos.  
  
### <a name="virtualization"></a>Virtualización  
Siempre que sea posible, considere la posibilidad de implementar máquinas virtuales, como hosts administrativos. Utiliza la virtualización, puede crear por usuario administrativos sistemas que se almacenan y se administran centralmente y que puede ser fácilmente apagan cuando no esté en uso, lo que garantiza que las credenciales no permanecen activas en los sistemas de administración. También puede requerir que los hosts administrativos virtuales se restablecen a una instantánea inicial después de cada uso, lo que garantiza que las máquinas virtuales permanecen en buen estadas. En la sección siguiente, se proporciona más información acerca de las opciones para la virtualización de hosts administrativos.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Enfoques de ejemplo para implementar Secure Hosts administrativos  
Independientemente de cómo diseñar e implementar la infraestructura de host administrativo, debe tener en cuenta las instrucciones especificadas en "Principios para crear Secure Hosts administrativos" anteriormente en este tema. Cada uno de los enfoques descritos aquí proporciona información general acerca de cómo puede separar "administrativo" y los sistemas "productivity" utilizados por el personal de TI. Los sistemas de productividad son equipos que los administradores de TI que se emplean para comprobar el correo electrónico, explorar Internet y usar software de productividad general como Microsoft Office. Sistemas administrativos son equipos que están protegidos y dedicados para usar para la administración diaria de un entorno de TI.  
  
La manera más sencilla de implementar los hosts administrativos seguros es proporcionar el personal de TI con estaciones de trabajo protegidas desde la que puede realizar tareas administrativas. En una implementación sólo estación de trabajo, cada estación de trabajo administrativa se utiliza para iniciar las herramientas de administración y las conexiones RDP para administrar servidores y otras infraestructuras. Implementaciones de sólo estación de trabajo pueden ser eficaces para las organizaciones más pequeñas, aunque más grandes y complejas infraestructuras pueden beneficiarse de un diseño distribuido para hosts administrativos en el que dedicado administrativos servidores y estaciones de trabajo se usan, como se describen en "Implementar administrativas estaciones de trabajo y saltar servidores seguros" más adelante en este tema.  
  
### <a name="implementing-separate-physical-workstations"></a>Implementación de estaciones de trabajo físicas independientes  
Una manera que puede implementar hosts administrativos es emitir a cada usuario de TI en dos estaciones de trabajo. Una estación de trabajo se usa con una cuenta de usuario "normal" para realizar actividades como comprobar el correo electrónico y el uso de aplicaciones de productividad, mientras que la segunda estación de trabajo está dedicado totalmente a las funciones administrativas.  
  
Para la estación de trabajo de productividad, el personal de TI puede tener derechos de cuentas de usuario normales en lugar de utilizar las cuentas con privilegios para iniciar sesión en equipos no protegidos. La estación de trabajo administrativa debe configurarse con una configuración rigurosamente controlada y el personal de TI debe usar una cuenta diferente para iniciar sesión en la estación de trabajo administrativa.  
  
Si ha implementado las tarjetas inteligentes, estaciones de trabajo administrativas deben configurarse para requerir que los inicios de sesión de tarjeta inteligente y personal de TI debe tener cuentas independientes para uso administrativo, que también se configura para requerir tarjetas inteligentes para el inicio de sesión interactivo. El host administrativo debe estar protegido como se describió anteriormente, y se deben permitir solo los usuarios de TI designados para iniciar sesión localmente en la estación de trabajo administrativa.  
  
#### <a name="pros"></a>Pros  
Al implementar sistemas físicos independientes, puede asegurarse de que cada equipo está configurado correctamente para su rol y que los usuarios de TI no pueden exponer sin darse cuenta sistemas administrativos a riesgos.  
  
#### <a name="cons"></a>Contras  
  
-   Implementación de equipos físicos diferentes aumenta los costos de hardware.  
  
-   Iniciar sesión en un equipo físico con las credenciales que se usan para administrar sistemas remotos se almacena en caché las credenciales en memoria.  
  
-   Si las estaciones de trabajo administrativas no se almacenan de forma segura, pueden ser vulnerables a poner en peligro a través de mecanismos como los registradores de claves de hardware físico u otros ataques físicos.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementación de una estación de trabajo físico seguro con una estación de trabajo virtualizadas de productividad  
En este enfoque, los usuarios de TI reciben una estación de trabajo administrativa segura desde el que pueden realizar funciones administrativas diarias, mediante las herramientas de administración remota de servidor (RSAT) o las conexiones RDP a servidores dentro de su ámbito de responsabilidad. Cuando los usuarios de TI necesitan realizar tareas de productividad, puede conectarse mediante RDP a una estación de trabajo de productividad remoto que se ejecuta como una máquina virtual. Se deben utilizar credenciales independientes para cada estación de trabajo, y deben implementarse controles como las tarjetas inteligentes.  
  
#### <a name="pros"></a>Pros  
  
-   Estaciones de trabajo de productividad y estaciones de trabajo administrativas están separados.  
  
-   Uso de estaciones de trabajo seguras para conectarse a las estaciones de trabajo de productividad de personal de TI puede usar credenciales separadas y tarjetas inteligentes y credenciales con privilegios no se depositan en el equipo menos seguras.  
  
#### <a name="cons"></a>Contras  
  
-   Implementación de la solución requiere el diseño y el trabajo de implementación y las opciones de virtualización sólido.  
  
-   Si las estaciones de trabajo físicos no se almacenan de forma segura, pueden ser vulnerables a ataques físicos que poner en peligro el hardware o el sistema operativo y que sean susceptibles de intercepción de comunicaciones.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementación de una estación de trabajo segura solo con las conexiones a separar "Productivity" y "Administrativo" máquinas virtuales  
En este enfoque, puede emitir una misma estación de trabajo físico que está bloqueado como se describió anteriormente, y en el que los usuarios de TI no tiene privilegios de acceso de usuarios de TI. Puede proporcionar las conexiones de servicios de escritorio remoto a máquinas virtuales hospedadas en servidores dedicados, que proporciona al personal de TI con una máquina virtual que ejecuta el correo electrónico y otras aplicaciones de productividad y una segunda máquina virtual que está configurada como el usuario host administrativo dedicado.  
  
Debe requerir tarjeta inteligente u otro inicio de sesión multifactor para las máquinas virtuales, con cuentas independientes que no sea la cuenta que se usa para iniciar sesión en el equipo físico. Después de que un usuario de TI inicia sesión en un equipo físico, puede usar su tarjeta inteligente de productividad para conectarse al equipo remoto de productividad y una cuenta independiente y tarjeta inteligente para conectarse a su equipo administrativo remoto.  
  
#### <a name="pros"></a>Pros  
  
-   Los usuarios de TI pueden usar una estación de trabajo físico único.  
  
-   Que requiere cuentas independientes para los hosts virtuales y el uso de conexiones de servicios de escritorio remoto a las máquinas virtuales, las credenciales de los usuarios de TI se almacenan en caché no en memoria en el equipo local.  
  
-   El host físico se puede proteger en la misma medida como hosts administrativos, reduciendo así la posibilidad de poner en peligro el equipo local.  
  
-   En los casos en que su máquina virtual administrativa o máquina virtual de un usuario TI productividad puede verse comprometida, la máquina virtual pueden restablecerse fácilmente a un estado "buena conocida".  
  
-   Si se pone en peligro el equipo físico, no hay credenciales con privilegios se almacenarán en caché en memoria y el uso de tarjetas inteligentes puede evitar poner en peligro las credenciales de registradores de pulsaciones de teclas.  
  
#### <a name="cons"></a>Contras  
  
-   Implementación de la solución requiere el diseño y el trabajo de implementación y las opciones de virtualización sólido.  
  
-   Si las estaciones de trabajo físicos no se almacenan de forma segura, pueden ser vulnerables a ataques físicos que poner en peligro el hardware o el sistema operativo y que sean susceptibles de intercepción de comunicaciones.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementación de estaciones de trabajo administrativas seguras y servidores de salto  
Como alternativa para proteger las estaciones de trabajo administrativas, o en combinación con ellos, puede implementar servidores de salto segura y a los usuarios administrativos pueden conectarse a los servidores de salto mediante RDP y tarjetas inteligentes para realizar tareas administrativas.  
  
Servidores de salto deben configurarse para ejecutar el rol de puerta de enlace de escritorio remoto para que pueda implementar restricciones en las conexiones para el servidor de salto y los servidores de destino que se administrará de él. Si es posible, también debe instalar el rol de Hyper-V y crear [escritorios virtuales personales](https://technet.microsoft.com/library/dd759174.aspx) u otras máquinas virtuales de cada usuario para los usuarios administrativos que se usará para sus tareas en los servidores de salto.  
  
Al asignar las máquinas virtuales de cada usuario a los usuarios administrativos en el servidor de salto, proporcionan seguridad física de las estaciones de trabajo administrativas, y los usuarios administrativos pueden restablecer o apagar sus máquinas virtuales cuando no esté en uso. Si prefiere no instalar el rol de Hyper-V y el rol de puerta de enlace de escritorio remoto en el mismo host administrativo, se puede instalar en equipos independientes.  
  
Siempre que sea posible, se deben usar herramientas de administración remota para administrar servidores. La característica herramientas de administración remota del servidor (RSAT) debe instalarse en máquinas virtuales de los usuarios (o el servidor de salto, si no va a implementar las máquinas virtuales de cada usuario para la administración) y personal administrativo debe conectarse a través de RDP a su máquinas virtuales para realizar tareas administrativas.  
  
En los casos cuando un usuario administrativo debe conectarse a través de RDP a un servidor de destino para administrarla directamente, puerta de enlace de escritorio remoto debe configurarse para permitir que la conexión se realiza solo si el usuario apropiado y el equipo se utilizan para establecer la conexión con el destino servidor. Ejecución de RSAT (o similar) herramientas deben prohibirse en sistemas que no estén designados sistemas de administración, tales como estaciones de trabajo de uso general y los servidores miembro que son servidores de salto no.  
  
#### <a name="pros"></a>Pros  
  
-   Creación de servidores de salto permite asignar servidores específicos a "zonas" (colecciones de sistemas con los requisitos de configuración, conexión y seguridad similares) en la red y requerir que la administración de cada zona se logra al personal administrativo conectarse desde los hosts administrativos seguros a un servidor designado "zone".  
  
-   Mediante la asignación de servidores de salto a zonas, puede implementar controles granulares para propiedades de conexión y los requisitos de configuración y puede identificar fácilmente los intentos de conexión de sistemas no autorizados.  
  
-   Mediante la implementación de máquinas virtuales de por administradores en los servidores de salto, forzar el apagado y restablecimiento de las máquinas virtuales a un estado limpio conocido cuando se completan las tareas administrativas. Mediante la imposición de cierre (o un reinicio) de las máquinas virtuales cuando se completan las tareas administrativas, las máquinas virtuales no puede destinarse a los atacantes, ni son ataques de robo de credenciales factible porque las credenciales de la memoria caché no persisten después de reiniciar el equipo.  
  
#### <a name="cons"></a>Contras  
  
-   Servidores dedicados son necesarios para servidores de salto, ya sea físico o virtual.  
  
-   Implementar designado saltar los servidores y estaciones de trabajo administrativas requiere una cuidadosa planeación y configuración que se asigna a las zonas de seguridad configuradas en el entorno.  
  


