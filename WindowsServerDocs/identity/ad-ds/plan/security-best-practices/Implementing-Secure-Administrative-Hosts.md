---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: "Implementación de Hosts administrativos seguros"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 42be89311f11ecc6a967b8b40600b53311253b89
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-secure-administrative-hosts"></a>Implementación de Hosts administrativos seguros

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seguros hosts administrativos son estaciones de trabajo o servidores que se han configurado específicamente para creación de plataformas de seguridad desde el que cuentas con privilegios pueden realizar tareas administrativas en Active Directory o en los controladores de dominio, sistemas unidos a un dominio y aplicaciones que se ejecutan en sistemas unidos a un dominio. En este caso, "cuentas con privilegios" hace referencia no solo a las cuentas que son miembros de los grupos más con privilegios en Active Directory, sino también a cualquier cuentas que han sido delegados los derechos y permisos que permiten que se deben realizar las tareas administrativas.  
  
Estas cuentas pueden ser cuentas de servicio de asistencia que tienen la capacidad para restablecer las contraseñas para la mayoría de los usuarios en un dominio, las cuentas que se usan para administrar los registros DNS y las zonas o con cuentas que se usan para la administración de la configuración. Seguros hosts administrativos se dedican a funciones administrativas y que no ejecuten software como aplicaciones de correo electrónico, los exploradores web o software de productividad como Microsoft Office.  
  
Aunque las cuentas y grupos "más privilegiadas" según corresponda deben ser el más estrictas protegido, esto no elimina la necesidad de proteger las cuentas y grupos a qué privilegios superiores a los de usuario estándar se han concedido cuentas.  
  
Un host administrativo seguro puede ser una estación de trabajo dedicado que se usa solo para las tareas administrativas, un servidor miembro que ejecuta el rol de servidor de puerta de enlace de escritorio remoto y a los que los usuarios de TI conectan para llevar a cabo, o un servidor que ejecuta el rol de Hyper-V y proporciona una máquina virtual única para cada usuario de TI que se usará para sus tareas administrativas de administración de hosts de destino. En muchos entornos, se pueden implementar combinaciones de todos los tres enfoques.  
  
La implementación de hosts administrativos seguros requiere la planeación y la configuración que sea coherente con el tamaño de la organización, las prácticas administrativas, gustos de riesgo y presupuesto. Consideraciones y opciones para la implementación de hosts administrativos seguros aquí se ofrecen para que su uso en desarrollar una estrategia administrativa adecuada para la organización.  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>Principios para la creación de Hosts administrativos seguros  
Para proteger los sistemas frente a ataques de forma eficaz, algunos principios generales deben tenerse en cuenta:  
  
1.  Nunca debe administrar un sistema de confianza (es decir, un servidor seguro como un controlador de dominio) desde un host de menor confianza (es decir, una estación de trabajo que no es segura para el mismo grado que los sistemas que administra).  
  
2.  No debe confiar en un factor de autenticación solo cuando se lleve a cabo actividades privilegiadas; es decir, las combinaciones de nombre y la contraseña de usuario no deben considerarse autenticación aceptable porque se representa solo un factor único (algo que conozcas). Debes tener en cuenta dónde credenciales ni se generan y almacena en caché almacenadas en escenarios de administración.  
  
3.  Aunque la mayoría de los ataques en el panorama actual de amenaza aprovecha el malware y piratería malintencionadas, no omitir la seguridad física, al diseñar e implementar hosts administrativos seguros.  
  
### <a name="account-configuration"></a>Configuración de la cuenta  
Incluso si la organización no usa actualmente las tarjetas inteligentes, considera la posibilidad de implementarlos para las cuentas con privilegios y seguros hosts administrativos. Hosts administrativos deben configurarse para requerir inicio de sesión de tarjeta inteligente para todas las cuentas mediante la modificación de la siguiente configuración en un GPO que esté vinculada a las unidades organizativas que contienen hosts administrativos:  
  
**Inicio de sesión de equipo equipo\Directivas\Configuración seguridad\Directivas locales\Opciones seguridad\Inicio: requerir tarjeta inteligente**  
  
Esta configuración requerirá todos los inicios de sesión interactivos para usar una tarjeta inteligente, independientemente de la configuración en una cuenta individual en Active Directory.  
  
También debes configurar seguros hosts administrativos para permitir que los inicios de sesión solo por las cuentas autorizadas, que se pueden configurar en:  
  
**Equipo equipo\Directivas\Configuración seguridad\Directivas locales\Opciones seguridad\Directivas Locales\asignación de derechos**  
  
Esto concede interactivo (y, en su caso, servicios de escritorio remoto) derechos de inicio de sesión solo a los usuarios autorizados del host administrativo seguro.  
  
### <a name="physical-security"></a>Seguridad física  
Para que se consideran de confianza administrativos hosts, debe configurarse y protegidos en el mismo grado que los sistemas que administran. La mayoría de las recomendaciones proporcionadas en [protección de controladores de dominio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) también son aplicables a los hosts que se usan para administrar controladores de dominio y la base de datos de AD DS. Uno de los desafíos de la implementación de sistemas de administración seguros en la mayoría de los entornos es que la seguridad física puede ser más difícil de implementar, ya que estos equipos a menudo se encuentran en áreas que no son tan seguras como servidores que hospedan en los centros de datos, como equipos de escritorio de los usuarios administrativos.  
  
Seguridad física incluye el control del acceso físico a los hosts administrativos. En una organización pequeña, esto puede significar que mantengas una estación de trabajo dedicado que se mantiene bloqueado en una oficina o de un cajón del escritorio cuando no está en uso. O bien, puede significar que cuando es necesario realizar la administración de Active Directory o los controladores de dominio, iniciar sesión en el controlador de dominio directamente.  
  
En medianas empresas, puedes considerar implementar seguro administrativas "salto servidores" que se encuentran en una ubicación segura en una oficina y se usan cuando se necesita una administración de Active Directory o controladores de dominio. También se pueden implementar administrativas estaciones de trabajo que están bloqueados en ubicaciones seguras cuando no está en uso, con o sin servidores de accesos directos.  
  
En las grandes empresas, puedes implementar los servidores de accesos directos aloja datacenter que proporcionan acceso controlado estrictamente a Active Directory; controladores de dominio. y servidores de archivos, imprimir o aplicación. Implementación de una arquitectura de servidor de accesos directos suele incluir una combinación de servidores y estaciones de trabajo seguras en entornos grandes.  
  
Independientemente del tamaño de la organización y el diseño de los hosts administrativos, debe proteger equipos físicos contra el acceso no autorizado o el robo y debe usar cifrado de unidad BitLocker para cifrar y proteger las unidades de hosts administrativas. Al implementar BitLocker en hosts administrativos, incluso si se roba un host o se quitan los discos, puedes asegurarte de que los datos de la unidad están inaccesibles para los usuarios no autorizados.  
  
### <a name="operating-system-versions-and-configuration"></a>Configuración y las versiones del sistema operativo  
Todos los hosts administrativos, si servidores o estaciones de trabajo, debe ejecutar el sistema operativo más reciente en el uso de la organización por los motivos que se describió anteriormente en este documento. Mediante la ejecución de los sistemas operativos actuales, tus beneficios personal administrativo de nuevas características de seguridad, soporte técnico del proveedor completa y funcionalidad adicional que se introdujo en el sistema operativo. Además, cuando va a evaluar un nuevo sistema operativo, mediante la implementación de primer lugar a los hosts administrativos, deberás familiarizarte con las nuevas características, configuración y los mecanismos de administración que ofrece, que posteriormente se pueden aprovechar en la planeación de implementación más amplia del sistema operativo. A continuación, los usuarios más sofisticados en la organización también estará los usuarios que están familiarizado con el nuevo sistema operativo y la mejor ubicación para soportarlo.  
  
### <a name="microsoft-security-configuration-wizard"></a>Asistente para configuración de seguridad de Microsoft  
Si implementas los servidores de accesos directos como parte de tu estrategia de host administrativas, debes usar al Asistente para configuración de seguridad integrada para configurar el servicio, el registro, auditoría y configuración del firewall para reducir la superficie de ataque del servidor. Cuando las opciones de configuración del Asistente para configuración de seguridad han se han recopilado y configurado, la configuración se puede convertir a un GPO que se usa para aplicar una configuración de línea base coherente en todos los servidores de accesos directos. Aún más se puede modificar el GPO para implementar específicos de configuración de seguridad para pasar los servidores y la posibilidad de combinar toda la configuración con la configuración de línea base adicionales extraído de Microsoft Security Compliance Manager.  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
La [Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx) es una herramienta gratuita que se integra las configuraciones de seguridad que se recomiendan por Microsoft, en función de la configuración de versión y la función del sistema operativo, y les recopila en una sola herramienta y la interfaz de usuario que puede usarse para crear y configurar la configuración de seguridad de línea base para controladores de dominio. Plantillas de Microsoft Security Compliance Manager pueden combinarse con la configuración del Asistente para configuración de seguridad para producir líneas de base de configuración completo para los servidores de accesos directos que se han implementado y aplican a los GPO implementados en las unidades organizativas en los accesos directos se encuentran los servidores de Active Directory.  
  
> [!NOTE]  
> Momento de redactar este documento, Microsoft Security Compliance Manager no incluye la configuración específica de los servidores de accesos directos u otros hosts administrativas seguros, pero aún puede usarse para crear líneas de base iniciales para los hosts administrativos Security Compliance Manager (SCM). Sin embargo, para proteger correctamente los hosts, debes aplicar otras configuraciones de seguridad adecuados a los servidores y estaciones de trabajo muy seguras.  
  
### <a name="applocker"></a>AppLocker  
Hosts administrativos y machinesshould virtual configurarse con el script, la herramienta y blancas de la aplicación a través de AppLocker o un software de restricción de aplicación de terceros. Todas las aplicaciones administrativas o utilidades que no se adhieren para proteger la configuración deben actualizarse o reemplazarse con herramientas que se adhiere al desarrollo seguro y las prácticas administrativas. Cuando se necesita herramientas nuevas o adicionales en un host administrativo, aplicaciones y las utilidades que se deben probar minuciosamente y, si es adecuada para su implementación en hosts administrativos las herramientas, pueden agregarse a blancas de los sistemas.  
  
### <a name="rdp-restrictions"></a>Restricciones de RDP  
Aunque la configuración específica puede variar según la arquitectura de los sistemas administrativas, debes incluir restricciones en el que cuentas y equipos pueden usarse para establecer conexiones de protocolo de escritorio remoto (RDP) con sistemas administrados, como el uso de puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto) accesos directos de servidores para controlar el acceso a controladores de dominio y otros administrado sistemas de los usuarios autorizados y sistemas.  
  
Deben permitir inicios de sesión interactivos para los usuarios autorizados y debe quitar o incluso bloquear otros tipos de inicio de sesión que no sean necesarias para el acceso al servidor.  
  
### <a name="patch-and-configuration-management"></a>Administración de revisiones y configuración  
Las organizaciones pequeñas pueden basarse en ofertas como Windows Update o [Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) (WSUS) para administrar la implementación de actualizaciones en sistemas de Windows, mientras que las organizaciones más grandes pueden implementar la configuración y revisión de software de administración empresarial, como System Center Configuration Manager. Independientemente de los mecanismos que usan para implementar actualizaciones en el servidor general y la población de la estación de trabajo, debes tener distintas implementaciones de sistemas de alta seguridad como controladores de dominio, entidades de certificación y hosts administrativos. Al separar estos sistemas de la infraestructura de administración general, si se pone en peligro las cuentas de servicio o software de administración, ponga en peligro no puede fácilmente extenderse a los sistemas más seguros en tu infraestructura.  
  
Aunque no debería implementar procesos de actualización manual para sistemas seguros, debe configurar una infraestructura aparte para actualizar los sistemas seguros. Incluso en organizaciones muy grandes, esta infraestructura puede implementarse normalmente a través de los servidores WSUS y GPO para sistemas protegidos.  
  
### <a name="blocking-internet-access"></a>Bloquear el acceso a Internet  
No deben permitirse hosts administrativos para tener acceso a Internet, ni se debe capaz de explorar la intranet de una organización. Los exploradores Web y aplicaciones similares no esté permitidas en hosts administrativos. Puede bloquear el acceso a Internet para hosts seguros mediante una combinación de configuración del firewall perímetro, la configuración de WFAS y "agujero negro" configuración de proxy en hosts seguros. También puedes usar la lista blanca de la aplicación para impedir que los exploradores web que se utilizan en hosts administrativos.  
  
### <a name="virtualization"></a>Virtualización  
Cuando sea posible, considera la posibilidad de implementación de máquinas virtuales como hosts administrativos. Usando la virtualización, puedes crear por usuario administrativos sistemas de forma centralizada que se almacena y administra y que puede fácilmente apagarse cuando no esté en uso, garantizar que las credenciales no quedan activas en los sistemas de administración. También puede requerir que los hosts administrativos virtuales se restablecen a una instantánea inicial después de cada uso, garantizar que las máquinas virtuales siguen siendo impecables. Para obtener más información sobre las opciones de virtualización de hosts administrativas se proporciona en la siguiente sección.  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Enfoques de muestra para la implementación de Secure Hosts administrativos  
Independientemente de cómo diseñar e implementar la infraestructura de host administrativas, debe tener en cuenta las directrices incluidas en "Principios para crear seguro administrativas Hosts" anteriormente en este tema. Cada uno de los enfoques descritos aquí proporciona información general acerca de cómo puedes separar "administrativas" y sistemas de "productividad" usados por el personal de TI. Los sistemas de productividad son equipos que los administradores de TI usan para comprobar el correo electrónico, navegar por Internet y cómo usar el software de productividad generales, como Microsoft Office. Sistemas administrativos son equipos que se destinó para usar administración diaria de un entorno de TI y reforzados.  
  
La forma más sencilla de implementar hosts administrativos seguros es proporcionar el personal de TI con protegido estaciones de trabajo desde el que pueden realizar tareas administrativas. En una implementación solo estación de trabajo, cada estación de trabajo se usa para iniciar herramientas de administración y las conexiones RDP para administrar los servidores y otras infraestructuras. Implementaciones solo estación de trabajo pueden ser eficaces para organizaciones más pequeñas, aunque infraestructuras más grandes y más complejas pueden beneficiarse de un diseño distribuido para hosts administrativos en el que se usan administrativos servidores y estaciones de trabajo, como se describe en "Implementar seguro administrativas estaciones de trabajo y accesos directos servidores" más adelante en este tema.  
  
### <a name="implementing-separate-physical-workstations"></a>Implementación de las estaciones de trabajo físicas independientes  
Es una forma que se pueden implementar hosts administrativos emitir dos estaciones de trabajo de cada usuario de TI. Una estación de trabajo se usa con una cuenta de usuario "normal" para llevar a cabo actividades como comprobar el correo electrónico y el uso de aplicaciones de productividad, mientras que la segunda estación de trabajo dedicado estrictamente a funciones administrativas.  
  
Para la estación de trabajo de productividad, el personal de TI puede tener cuentas de usuario normal, en lugar de usar cuentas con privilegios para iniciar sesión en equipos que no sean seguros. La estación de trabajo administrativa debe estar configurado con una configuración estrictas controlada y el personal de TI debe usar una cuenta diferente para iniciar sesión en la estación de trabajo.  
  
Si se han implementado tarjetas inteligentes, estaciones de trabajo administrativas deben configurarse para solicitar los inicios de sesión de tarjeta inteligente y el personal de TI debe tener cuentas separadas para el uso administrativo, también está configurado para requerir tarjetas inteligentes para el inicio de sesión interactivo. El host administrativo debe ser reforzado como se describió anteriormente, y solo los usuarios de TI designados deben poder iniciar sesión localmente en la estación de trabajo.  
  
#### <a name="pros"></a>Profesionales de TI  
Al implementar sistemas físicos independientes, puedes asegurarte de que cada equipo está configurado correctamente para su función y que los usuarios de TI no pueden exponer sin darse cuenta sistemas administrativos de riesgo.  
  
#### <a name="cons"></a>Desventajas  
  
-   Implementar equipos físicos independientes aumenta los costos de hardware.  
  
-   Inicio de sesión en un equipo físico con las credenciales que se usan para administrar sistemas remotos, se almacena en caché las credenciales en la memoria.  
  
-   Si las estaciones de trabajo administrativas no se almacenan de forma segura, es posible que sea vulnerables a poner en riesgo a través de mecanismos como pulsaciones de teclas de hardware físico o de otros ataques físicos.  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementar una estación de trabajo físico seguro con una estación de trabajo de productividad virtualizada  
En este enfoque, los usuarios de TI se proporcionan una estación de trabajo administrativa protegido desde el que pueden realizar funciones administrativas diarias, con herramientas de administración remota del servidor (RSAT) o las conexiones RDP Emitirán a los servidores de su ámbito de responsabilidad. Cuando los usuarios de TI necesitan para llevar a cabo tareas de productividad, puede conectar a través de RDP una estación de trabajo de productividad remoto que se ejecuta como una máquina virtual. Deben usarse varias credenciales distintas para cada estación de trabajo, y controles, como tarjetas inteligentes deben implementarse.  
  
#### <a name="pros"></a>Profesionales de TI  
  
-   Se separan estaciones de trabajo administrativas y estaciones de trabajo de productividad.  
  
-   Uso seguros estaciones de trabajo para conectar a estaciones de trabajo de productividad el personal de TI puede usar credenciales distintas y tarjetas inteligentes y no se depositan credenciales con privilegios en el equipo menos seguros.  
  
#### <a name="cons"></a>Desventajas  
  
-   Implementación de la solución requiere el diseño y el trabajo de implementación y las opciones de virtualización sólido.  
  
-   Si las estaciones de trabajo físicos no se almacenan de forma segura, es posible que sea vulnerables a ataques físicos que poner en peligro el hardware o el sistema operativo y hacen susceptibles de sufrir interceptación de comunicaciones.  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementar una estación de trabajo única segura con las conexiones a separar "Productividad" y "Administrativas" máquinas virtuales  
En este enfoque, puede emitir una sola estación de trabajo física que está bloqueada como se describió anteriormente, y en el que los usuarios de TI no tienen acceso con privilegios de los usuarios de TI. Puedes proporcionar las conexiones de servicios de escritorio remoto a equipos virtuales alojados en servidores, proporcionar personal de TI con una máquina virtual que se ejecuta de correo electrónico y otras aplicaciones de productividad y una segunda máquina virtual que esté configurada como host administrativas dedicado del usuario.  
  
Deberías requerir tarjeta inteligente u otro multifactor inicio de sesión para las máquinas virtuales, usan cuentas separadas aparte de la cuenta que se usa para iniciar sesión en el equipo físico. Después de que un usuario de TI inicia sesión en un equipo físico, pueden usar su tarjeta inteligente de productividad para conectarse a su equipo remoto productividad y una cuenta independiente y la tarjeta inteligente para conectarse a su equipo remoto administrativa.  
  
#### <a name="pros"></a>Profesionales de TI  
  
-   Los usuarios de TI pueden usar una estación de trabajo única físico.  
  
-   Que requieren cuentas separadas para los hosts virtuales y usar conexiones de servicios de escritorio remoto para las máquinas virtuales, las credenciales de los usuarios de TI se almacenan en caché no en la memoria en el equipo local.  
  
-   El host físico puede colocarse en el mismo grado como hosts administrativos, reduciendo la posibilidad de peligro del equipo local.  
  
-   En casos en que sus administrativa máquina virtual o máquina virtual de productividad de un usuario TI puede haberse puesto en riesgo, la máquina virtual se puede restablecer fácilmente a un estado "buena conocida".  
  
-   Si el equipo físico se ve comprometido, credenciales con privilegios no se almacenará en caché en la memoria y el uso de tarjetas inteligentes, puedes impedir el compromiso de credenciales mediante el teclado de pulsaciones de teclas.  
  
#### <a name="cons"></a>Desventajas  
  
-   Implementación de la solución requiere el diseño y el trabajo de implementación y las opciones de virtualización sólido.  
  
-   Si las estaciones de trabajo físicos no se almacenan de forma segura, es posible que sea vulnerables a ataques físicos que poner en peligro el hardware o el sistema operativo y hacen susceptibles de sufrir interceptación de comunicaciones.  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementación de accesos directos servidores y estaciones de trabajo administrativas seguros  
Como alternativa para proteger las estaciones de trabajo administrativas, o en combinación con ellos, puedes implementar los servidores de accesos directos segura y de los usuarios administrativos pueden conectarse a los servidores de accesos directos con RDP y tarjetas inteligentes para realizar tareas administrativas.  
  
Servidores de accesos directos deben estar configurados para ejecutar la función de la puerta de enlace de escritorio remoto para que pueda implementar restricciones en las conexiones al servidor de accesos directos y a los servidores de destino que se administrarán de ella. Si es posible, debe instalar el rol de Hyper-V y crear también [escritorios virtuales personales](https://technet.microsoft.com/library/dd759174.aspx) u otras máquinas virtuales de cada usuario para que los usuarios administrativos utilizan para sus tareas en los servidores de accesos directos.  
  
Al conceder a los usuarios administrativos máquinas virtuales de cada usuario en el servidor de accesos directos, proporcionar seguridad física para las estaciones de trabajo administrativas y de los usuarios administrativos restablecer o apagar las máquinas virtuales cuando no está en uso. Si prefieres no instalar el rol de Hyper-V y la función de la puerta de enlace de escritorio remoto en el mismo host administrativo, se puede instalar en equipos separados.  
  
Siempre que sea posible, herramientas de administración remota deben usarse para administrar los servidores. La característica de herramientas de administración remota del servidor (RSAT) debe instalarse en máquinas virtuales de los usuarios (o el servidor si no estás implementando máquinas virtuales de cada usuario para la administración de accesos directos) y personal administrativo debe conectarse a través de RDP para las máquinas virtuales para realizar tareas administrativas.  
  
En los casos cuando un usuario administrador debe conectarse a través de RDP a un servidor de destino a administrar directamente, puerta de enlace de escritorio remoto debe configurarse para permitir que la conexión realizarse únicamente si el usuario apropiado y el equipo se usan para establecer la conexión con el servidor de destino. Ejecución de RSAT (o similar) herramientas deberían prohibirse en sistemas que no se indican los sistemas de administración, como las estaciones de trabajo de uso general y los servidores miembro que son accesos directos no servidores.  
  
#### <a name="pros"></a>Profesionales de TI  
  
-   Crear accesos directos servidores permite asignar a determinados servidores "las zonas de" (colecciones de sistemas con los requisitos de configuración, conexión y seguridad similares) en la red y requieren que la administración de cada zona se logra al personal administrativo conectarse desde hosts administrativos seguros a un servidor designado "zona".  
  
-   Mediante la asignación de servidores de accesos directos a las zonas, pueden implementar controles granulares para conocer los requisitos de configuración y propiedades de conexión y puede identificar fácilmente los intentos de conexión de sistemas no autorizados.  
  
-   Mediante la implementación de máquinas virtuales de por administrador en los servidores de accesos directos, aplicar apagado y restablecimiento de las máquinas virtuales a un estado conocido limpia cuando se completen las tareas administrativas. Al exigir apagado (o reinicio) de las máquinas virtuales cuando se completen las tareas administrativas, las máquinas virtuales no puede ser el objetivo los atacantes, ni tampoco ataques de robo de credenciales factible porque no se mantienen las credenciales en la memoria caché más allá de un reinicio.  
  
#### <a name="cons"></a>Desventajas  
  
-   Servidores son necesarios para servidores de accesos directos, ya sea física o virtual.  
  
-   Implementar designado accesos directos a servidores y estaciones de trabajo administrativas requiere un diseño cuidadoso y configuración que se asigna a las zonas de seguridad configuradas en el entorno.  
  


