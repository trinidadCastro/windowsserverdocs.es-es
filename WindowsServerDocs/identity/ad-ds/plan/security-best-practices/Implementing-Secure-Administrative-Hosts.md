---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: Implementación de hosts administrativos seguros
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a9b73fb5f62ce9953a5be989e1b99bebf6c9c3e3
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069604"
---
# <a name="implementing-secure-administrative-hosts"></a>Implementación de hosts administrativos seguros

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los hosts administrativos seguros son estaciones de trabajo o servidores que se han configurado específicamente con el fin de crear plataformas seguras a partir de las cuales las cuentas con privilegios pueden realizar tareas administrativas en Active Directory o en controladores de dominio, sistemas Unidos a un dominio y aplicaciones que se ejecutan en sistemas Unidos a un dominio. En este caso, "cuentas con privilegios" no solo hace referencia a las cuentas que son miembros de los grupos con más privilegios en Active Directory, sino a cualquier cuenta a la que se haya delegado derechos y permisos que permitan realizar tareas administrativas.

Estas cuentas pueden ser cuentas del Departamento de soporte técnico que tienen la capacidad de restablecer contraseñas de la mayoría de los usuarios de un dominio, las cuentas que se usan para administrar los registros y las zonas DNS, o las cuentas que se usan para la administración de configuración. Los hosts administrativos seguros están dedicados a la funcionalidad administrativa y no ejecutan software como aplicaciones de correo electrónico, exploradores Web o software de productividad como Microsoft Office.

Aunque las cuentas y los grupos "con más privilegios" deberían ser, en consecuencia, los más estrictos, esto no elimina la necesidad de proteger las cuentas y los grupos a los que se han concedido privilegios por encima de los de las cuentas de usuario estándar.

Un host administrativo seguro puede ser una estación de trabajo dedicada que solo se usa para tareas administrativas, un servidor miembro que ejecuta el rol de servidor de puerta de enlace Escritorio remoto y al que los usuarios se conectan para llevar a cabo la administración de hosts de destino, o un servidor que ejecuta el rol de Hyper-V y proporciona una máquina virtual única para que lo use cada usuario de ti. En muchos entornos, se pueden implementar combinaciones de los tres enfoques.

La implementación de hosts administrativos seguros requiere planeación y configuración que sea coherente con el tamaño de la organización, las prácticas administrativas, el apetito de riesgo y el presupuesto. Aquí se proporcionan consideraciones y opciones para la implementación de hosts administrativos seguros para su uso en el desarrollo de una estrategia administrativa adecuada para su organización.

## <a name="principles-for-creating-secure-administrative-hosts"></a>Principios para la creación de hosts administrativos seguros
Para proteger eficazmente los sistemas frente a ataques, deben tenerse en cuenta algunos principios generales:

1.  Nunca debe administrar un sistema de confianza (es decir, un servidor seguro como un controlador de dominio) desde un host de menos confianza (es decir, una estación de trabajo que no está protegida al mismo grado que los sistemas que administra).

2.  No debe basarse en un único factor de autenticación al realizar actividades con privilegios; es decir, las combinaciones de nombre de usuario y contraseña no deben considerarse una autenticación aceptable porque solo se representa un único factor (algo que sabe). Debe considerar dónde se generan y almacenan en caché las credenciales, o se almacenan en escenarios administrativos.

3.  Aunque la mayoría de los ataques en el panorama de amenazas actual aprovechan el malware y la piratería malintencionada, no omita la seguridad física al diseñar e implementar hosts administrativos seguros.

### <a name="account-configuration"></a>Configuración de la cuenta
Incluso si su organización no usa tarjetas inteligentes actualmente, debe considerar la posibilidad de implementarlas para las cuentas con privilegios y los hosts administrativos seguros. Los hosts administrativos deben configurarse para requerir el inicio de sesión de tarjeta inteligente para todas las cuentas mediante la modificación de la configuración siguiente en un GPO vinculado a las unidades organizativas que contienen hosts administrativos:

**Equipo \ directivas de Seguridad\directivas Locales\opciones de Options\Interactive inicio de sesión: requerir tarjeta inteligente**

Esta configuración requerirá que todos los inicios de sesión interactivos usen una tarjeta inteligente, independientemente de la configuración de una cuenta individual en Active Directory.

También debe configurar hosts administrativos seguros para permitir los inicios de sesión solo por cuentas autorizadas, que se pueden configurar en:

**Equipo \ directivas de Seguridad\directivas Locales\opciones de seguridad\Directivas locales \ asignación de derechos**

Esto concede derechos de inicio de sesión interactivos (y, si es necesario Servicios de Escritorio remoto) solo a los usuarios autorizados del host administrativo seguro.

### <a name="physical-security"></a>Seguridad física
Para que los hosts administrativos se consideren de confianza, deben configurarse y protegerse en el mismo grado que los sistemas que administran. La mayoría de las recomendaciones proporcionadas en [protección de controladores de dominio contra ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) también se aplican a los hosts que se usan para administrar controladores de dominio y la base de datos de AD DS. Uno de los desafíos de la implementación de sistemas administrativos seguros en la mayoría de los entornos es que la seguridad física puede ser más difícil de implementar porque estos equipos suelen residir en áreas que no son tan seguras como servidores hospedados en los centros de recursos, como los escritorios de los usuarios administrativos.

La seguridad física incluye el control de acceso físico a los hosts administrativos. En una organización pequeña, esto puede significar que mantiene una estación de trabajo administrativa dedicada que se mantiene bloqueada en una oficina o un cajón de escritorio cuando no está en uso. O bien, puede significar que, cuando necesite realizar la administración de Active Directory o de los controladores de dominio, inicie sesión directamente en el controlador de dominio.

En organizaciones de tamaño medio, puede considerar la posibilidad de implementar "servidores de salto" administrativos seguros que se encuentran en una ubicación segura en una oficina y se usan cuando se requiere la administración de Active Directory o controladores de dominio. También puede implementar estaciones de trabajo administrativas que estén bloqueadas en ubicaciones seguras cuando no estén en uso, con o sin servidores de salto.

En organizaciones de gran tamaño, puede implementar servidores de saltos hospedados en centros de Datacenter que proporcionen acceso estrictamente controlado a Active Directory; Controladores de dominio; y servidores de archivos, de impresión o de aplicaciones. La implementación de una arquitectura de Jump Server es más probable que incluya una combinación de estaciones de trabajo y servidores seguros en entornos de gran tamaño.

Independientemente del tamaño de la organización y del diseño de los hosts administrativos, debe proteger los equipos físicos contra el acceso no autorizado o el robo, y debe utilizar Cifrado de unidad BitLocker para cifrar y proteger las unidades en los hosts administrativos. Mediante la implementación de BitLocker en hosts administrativos, incluso si se roba un host o se quitan los discos, puede asegurarse de que los datos de la unidad sean inaccesibles para usuarios no autorizados.

### <a name="operating-system-versions-and-configuration"></a>Versiones y configuración del sistema operativo
Todos los hosts administrativos, ya sean servidores o estaciones de trabajo, deben ejecutar el sistema operativo más reciente en uso en su organización por los motivos descritos anteriormente en este documento. Al ejecutar los sistemas operativos actuales, el personal administrativo se beneficia de las nuevas características de seguridad, la compatibilidad total con proveedores y la funcionalidad adicional introducida en el sistema operativo. Además, al evaluar un nuevo sistema operativo, mediante su implementación primero en los hosts administrativos, deberá familiarizarse con las nuevas características, la configuración y los mecanismos de administración que ofrece, que se pueden aprovechar posteriormente en la planificación de una implementación más amplia del sistema operativo. A continuación, los usuarios más sofisticados de la organización también serán los usuarios que estén familiarizados con el nuevo sistema operativo y que mejor se adapten a su compatibilidad.

### <a name="microsoft-security-configuration-wizard"></a>Asistente para configuración de seguridad de Microsoft
Si implementa servidores de saltos como parte de su estrategia de hosts administrativos, debe usar el Asistente para configuración de seguridad integrada para configurar el servicio, el registro, la auditoría y la configuración de Firewall para reducir la superficie expuesta a ataques del servidor. Cuando se recopilan y configuran los valores de configuración del Asistente para configuración de seguridad, la configuración se puede convertir en un GPO que se usa para aplicar una configuración de línea de base coherente en todos los servidores de salto. Puede modificar aún más el objeto de directiva de grupo para implementar la configuración de seguridad específica de los servidores de salto y puede combinar todas las opciones de configuración con una configuración de línea de base adicional extraída de Microsoft Security Compliance Manager.

### <a name="microsoft-security-compliance-manager"></a>Administrador de cumplimiento de seguridad de Microsoft
[Microsoft Security Compliance Manager](/previous-versions/tn-archive/cc677002(v=technet.10)) es una herramienta disponible gratuita que integra las configuraciones de seguridad recomendadas por Microsoft, según la versión del sistema operativo y la configuración de roles, y las recopila en una sola herramienta y interfaz de usuario que se pueden usar para crear y configurar las opciones de seguridad de línea de base para los controladores de dominio. Las plantillas del administrador de cumplimiento de seguridad de Microsoft se pueden combinar con la configuración del Asistente para configuración de seguridad para generar líneas de base de configuración completas para los servidores de salto implementados y aplicados por los GPO implementados en las unidades organizativas en las que se encuentran los servidores de salto en Active Directory.

> [!NOTE]
> En el que se redactó este documento, el administrador de cumplimiento de seguridad de Microsoft no incluye la configuración específica para los servidores de saltos u otros hosts administrativos seguros, pero el administrador de cumplimiento de seguridad (SCM) se puede seguir usando para crear líneas base iniciales para los hosts administrativos. Sin embargo, para proteger correctamente los hosts, debe aplicar una configuración de seguridad adicional adecuada a estaciones de trabajo y servidores altamente seguros.

### <a name="applocker"></a>AppLocker
Los hosts administrativos y las máquinas virtuales deben configurarse con scripts, herramientas y aplicaciones mediante AppLocker o un software de restricción de aplicaciones de terceros. Las aplicaciones administrativas o utilidades que no se adhieren a la configuración segura deben actualizarse o reemplazarse por herramientas que se adhieren a las prácticas administrativas y de desarrollo seguro. Cuando se necesitan herramientas nuevas o adicionales en un host administrativo, las aplicaciones y utilidades se deben probar minuciosamente y, si las herramientas son adecuadas para la implementación en los hosts administrativos, se pueden agregar a los sistemas.

### <a name="rdp-restrictions"></a>Restricciones de RDP
Aunque la configuración específica variará en función de la arquitectura de los sistemas administrativos, debe incluir restricciones en las cuentas y los equipos que se pueden usar para establecer conexiones Protocolo de escritorio remoto (RDP) con sistemas administrados, como el uso de servidores de salto de puerta de enlace de Escritorio remoto (puerta de enlace de escritorio remoto) para controlar el acceso a los controladores de dominio y otros sistemas administrados desde usuarios y sistemas

Debe permitir los inicios de sesión interactivos por parte de usuarios autorizados y quitar o incluso bloquear otros tipos de inicio de sesión que no sean necesarios para el acceso al servidor.

### <a name="patch-and-configuration-management"></a>Administración de revisiones y configuraciones
Las organizaciones más pequeñas pueden confiar en ofertas como Windows Update o [Windows Server Update Services](/windows/deployment/deploy-whats-new) (WSUS) para administrar la implementación de actualizaciones en sistemas Windows, mientras que las organizaciones más grandes pueden implementar software de administración de configuración y revisión empresarial, como Microsoft Endpoint Configuration Manager. Sin tener en cuenta los mecanismos que se usan para implementar actualizaciones en el servidor general y la población de la estación de trabajo, se deben considerar implementaciones independientes para sistemas altamente seguros, como controladores de dominio, entidades de certificación y hosts administrativos. Al separar estos sistemas de la infraestructura de administración general, si el software de administración o las cuentas de servicio están en peligro, el riesgo no se puede ampliar fácilmente a los sistemas más seguros de la infraestructura.

Aunque no debe implementar procesos de actualización manuales para sistemas seguros, debe configurar una infraestructura independiente para actualizar sistemas seguros. Incluso en organizaciones de gran tamaño, esta infraestructura normalmente se puede implementar a través de servidores WSUS dedicados y GPO para sistemas protegidos.

### <a name="blocking-internet-access"></a>Bloquear el acceso a Internet
Los hosts administrativos no deben tener permiso de acceso a Internet ni deben ser capaces de examinar la intranet de una organización. Los exploradores Web y las aplicaciones similares no deben permitirse en los hosts administrativos. Puede bloquear el acceso a Internet para hosts seguros a través de una combinación de configuración de firewall perimetral, configuración de WFAS y configuración de proxy de "agujero negro" en hosts seguros. También puede usar la aplicación allowslist para evitar que se usen exploradores Web en hosts administrativos.

### <a name="virtualization"></a>Virtualización
Siempre que sea posible, considere la posibilidad de implementar máquinas virtuales como hosts administrativos. Con la virtualización, puede crear sistemas administrativos por usuario que se almacenan y administran de forma centralizada y que se pueden apagar fácilmente cuando no se usan, asegurándose de que las credenciales no se mantienen activas en los sistemas administrativos. También puede exigir que los hosts administrativos virtuales se restablezcan a una instantánea inicial después de cada uso, asegurándose de que las máquinas virtuales permanezcan imprecisas. En la sección siguiente se proporciona más información sobre las opciones de virtualización de hosts administrativos.

## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>Métodos de ejemplo para implementar hosts administrativos seguros
Independientemente de cómo diseñe e implemente la infraestructura del host administrativo, debe tener en cuenta las instrucciones proporcionadas en "principios para la creación de hosts administrativos seguros", anteriormente en este tema. Cada uno de los enfoques descritos aquí proporciona información general acerca de cómo puede separar los sistemas de "administración" y "productividad" utilizados por el personal de ti. Los sistemas de productividad son equipos que los administradores de ti emplean para comprobar el correo electrónico, explorar Internet y usar software de productividad general como Microsoft Office. Los sistemas administrativos son equipos protegidos y dedicados a su uso para la administración diaria de un entorno de ti.

La manera más sencilla de implementar hosts administrativos seguros es proporcionar a su personal de ti estaciones de trabajo seguras desde las que pueden realizar tareas administrativas. En una implementación de solo estación de trabajo, cada estación de trabajo administrativa se usa para iniciar las herramientas de administración y las conexiones RDP para administrar servidores y otras infraestructuras. Las implementaciones de solo estación de trabajo pueden ser eficaces en organizaciones más pequeñas, aunque las infraestructuras más grandes y complejas pueden beneficiarse de un diseño distribuido para hosts administrativos en los que se usan servidores administrativos y estaciones de trabajo dedicados, como se describe en "implementación de estaciones de trabajo administrativas seguras y servidores de salto" más adelante en este tema.

### <a name="implementing-separate-physical-workstations"></a>Implementación de estaciones de trabajo físicas independientes
Una manera de implementar hosts administrativos es emitir dos estaciones de trabajo para cada usuario de ti. Una estación de trabajo se usa con una cuenta de usuario "normal" para realizar actividades como la comprobación del correo electrónico y el uso de aplicaciones de productividad, mientras que la segunda estación de trabajo está dedicada exclusivamente a las funciones administrativas.

En la estación de trabajo de productividad, el personal de ti puede tener cuentas de usuario normales en lugar de usar cuentas con privilegios para iniciar sesión en equipos no seguros. La estación de trabajo administrativa debe configurarse con una configuración controlada rigurosamente y el personal de TI debe utilizar una cuenta diferente para iniciar sesión en la estación de trabajo administrativa.

Si ha implementado tarjetas inteligentes, las estaciones de trabajo administrativas deben configurarse para requerir inicios de sesión de tarjeta inteligente y el personal de TI debe recibir cuentas independientes para uso administrativo, también configurada para requerir tarjetas inteligentes para el inicio de sesión interactivo. El host administrativo debe protegerse como se ha descrito anteriormente y solo los usuarios de ti designados deben tener permiso para iniciar sesión localmente en la estación de trabajo administrativa.

#### <a name="pros"></a>Ventajas
Mediante la implementación de sistemas físicos independientes, puede asegurarse de que cada equipo esté configurado correctamente para su rol y de que los usuarios de ti no puedan exponer accidentalmente los sistemas administrativos al riesgo.

#### <a name="cons"></a>Desventajas

-   La implementación de equipos físicos independientes aumenta los costos de hardware.

-   El inicio de sesión en un equipo físico con credenciales que se usan para administrar sistemas remotos almacena en caché las credenciales en memoria.

-   Si las estaciones de trabajo administrativas no se almacenan de forma segura, pueden ser vulnerables al riesgo a través de mecanismos como los registradores de claves de hardware físico u otros ataques físicos.

### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>Implementación de una estación de trabajo física segura con una estación de trabajo de productividad virtualizada
En este enfoque, se proporciona a los usuarios de ti una estación de trabajo administrativa segura desde la que pueden realizar funciones administrativas cotidianas, mediante el uso de Herramientas de administración remota del servidor (RSAT) o conexiones RDP a servidores dentro de su ámbito de responsabilidad. Cuando los usuarios necesitan realizar tareas de productividad, pueden conectarse a través de RDP a una estación de trabajo de productividad remota que se ejecuta como una máquina virtual. Se deben usar credenciales independientes para cada estación de trabajo y se deben implementar controles como tarjetas inteligentes.

#### <a name="pros"></a>Ventajas

-   Las estaciones de trabajo administrativas y las estaciones de trabajo de productividad están separadas.

-   El personal de ti que usa estaciones de trabajo seguras para conectarse a estaciones de trabajo de productividad puede usar credenciales y tarjetas inteligentes independientes, y las credenciales con privilegios no se depositan en el equipo menos seguro.

#### <a name="cons"></a>Desventajas

-   La implementación de la solución requiere un trabajo de diseño e implementación, así como opciones de virtualización sólidas.

-   Si las estaciones de trabajo físicas no se almacenan de forma segura, pueden ser vulnerables a ataques físicos que pongan en peligro el hardware o el sistema operativo y que sean susceptibles a la interceptación de comunicaciones.

### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>Implementación de una sola estación de trabajo segura con conexiones para separar la "productividad" y la "administración" Virtual Machines
En este enfoque, puede emitir a los usuarios una única estación de trabajo física que esté bloqueada como se ha descrito anteriormente y en la que los usuarios de ti no tienen privilegios de acceso. Puede proporcionar conexiones Servicios de Escritorio remoto a las máquinas virtuales hospedadas en servidores dedicados, lo que proporciona al personal de ti una máquina virtual que ejecuta el correo electrónico y otras aplicaciones de productividad, y una segunda máquina virtual configurada como el host administrativo dedicado del usuario.

Debe requerir una tarjeta inteligente u otro inicio de sesión multifactor para las máquinas virtuales, mediante cuentas independientes distintas de la cuenta que se usa para iniciar sesión en el equipo físico. Una vez que un usuario de ti inicia sesión en un equipo físico, puede usar su tarjeta inteligente de productividad para conectarse a su equipo de productividad remoto y una cuenta independiente y una tarjeta inteligente para conectarse a su equipo administrativo remoto.

#### <a name="pros"></a>Ventajas

-   Los usuarios de TI pueden usar una sola estación de trabajo física.

-   Al requerir cuentas independientes para los hosts virtuales y usar conexiones Servicios de Escritorio remoto a las máquinas virtuales, las credenciales de los usuarios de ti no se almacenan en la memoria caché del equipo local.

-   El host físico se puede proteger al mismo grado que los hosts administrativos, lo que reduce la probabilidad de poner en peligro el equipo local.

-   En los casos en los que la máquina virtual de productividad de un usuario de ti o su máquina virtual administrativa pueden estar en peligro, la máquina virtual puede restablecerse fácilmente a un estado conocido.

-   Si el equipo físico está en peligro, no se almacenarán en memoria caché las credenciales con privilegios y el uso de tarjetas inteligentes puede impedir el riesgo de credenciales por parte de los registradores de pulsaciones de teclas.

#### <a name="cons"></a>Desventajas

-   La implementación de la solución requiere un trabajo de diseño e implementación, así como opciones de virtualización sólidas.

-   Si las estaciones de trabajo físicas no se almacenan de forma segura, pueden ser vulnerables a ataques físicos que pongan en peligro el hardware o el sistema operativo y que sean susceptibles a la interceptación de comunicaciones.

### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>Implementación de estaciones de trabajo administrativas seguras y servidores de salto
Como alternativa para proteger las estaciones de trabajo administrativas o en combinación con ellas, puede implementar servidores de saltos seguros, y los usuarios administrativos pueden conectarse a los servidores de salto mediante RDP y tarjetas inteligentes para realizar tareas administrativas.

Los servidores de salto deben configurarse para ejecutar el rol de puerta de enlace de Escritorio remoto para que pueda implementar restricciones en las conexiones con el servidor de saltos y en los servidores de destino que se administrarán desde él. Si es posible, también debe instalar el rol de Hyper-V y crear [escritorios virtuales personales](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759174(v=ws.11)) u otras máquinas virtuales por usuario para que los usuarios administrativos puedan usar para sus tareas en los servidores de salto.

Al conceder a los usuarios administrativos máquinas virtuales por usuario en el servidor de Jump Server, se proporciona seguridad física para las estaciones de trabajo administrativas y los usuarios administrativos pueden restablecer o apagar sus máquinas virtuales cuando no estén en uso. Si prefiere no instalar el rol de Hyper-V y el rol de puerta de enlace de Escritorio remoto en el mismo host administrativo, puede instalarlos en equipos independientes.

Siempre que sea posible, las herramientas de administración remota deben usarse para administrar servidores. La característica Herramientas de administración remota del servidor (RSAT) debe instalarse en las máquinas virtuales de los usuarios (o en el servidor de salto Si no está implementando máquinas virtuales por usuario para la administración), y el personal administrativo debe conectarse a través de RDP a sus máquinas virtuales para realizar tareas administrativas.

En los casos en los que un usuario administrativo debe conectarse a través de RDP a un servidor de destino para administrarlo directamente, la puerta de enlace de escritorio remoto debe configurarse para permitir que se realice la conexión solo si el usuario y el equipo adecuados se usan para establecer la conexión al servidor de destino. La ejecución de herramientas de RSAT (o similares) debe prohibirse en sistemas que no son sistemas de administración designados, como las estaciones de trabajo de uso general y los servidores miembro que no son servidores de salto.

#### <a name="pros"></a>Ventajas

-   La creación de servidores de salto permite asignar servidores específicos a "zonas" (recopilaciones de sistemas con requisitos de configuración, conexión y seguridad similares) en la red y requerir que la administración de cada zona se lleve a cabo por el personal administrativo que se conecta desde hosts administrativos seguros a un servidor "zona" designado.

-   Al asignar servidores de salto a zonas, puede implementar controles granulares para las propiedades de conexión y los requisitos de configuración, y puede identificar fácilmente los intentos de conexión desde sistemas no autorizados.

-   Mediante la implementación de máquinas virtuales por administrador en servidores de saltos, se exige el cierre y el restablecimiento de las máquinas virtuales a un estado limpio conocido cuando se completan las tareas administrativas. Al exigir el cierre (o reinicio) de las máquinas virtuales cuando se completan las tareas administrativas, los atacantes no pueden destinar las máquinas virtuales, ni tampoco los ataques de robo de credenciales, ya que las credenciales almacenadas en memoria caché no se conservan después de un reinicio.

#### <a name="cons"></a>Desventajas

-   Los servidores dedicados son necesarios para los servidores de saltos, ya sean físicos o virtuales.

-   La implementación de servidores de salto y estaciones de trabajo administrativas designadas requiere un cuidadoso planeamiento y configuración que se asignan a cualquier zona de seguridad configurada en el entorno.

