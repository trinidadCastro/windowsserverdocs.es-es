---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Niveles funcionales de Windows Server 2016
description: 
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: a39955cf088ce7ce8bef20c70b83d49c6d508497
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="forest-and-domain-functional-levels"></a>Niveles funcionales de bosque y dominio

>Se aplica a: Windows Server

Con el final del ciclo de vida de Windows 2003, controladores de dominio de Windows 2003 (DC) deben actualizarse a Windows Server 2008, 2012 o 2016. Como resultado, debe quitarse cualquier controlador de dominio que ejecuta Windows Server 2003 del dominio. El nivel funcional del bosque y dominio debe emitirse para al menos Windows Server 2008 para evitar que un controlador de dominio que se ejecuta una versión anterior de Windows Server desde que se agregan en el entorno.

Te recomendamos que los clientes actualizar su nivel funcional del dominio (DFL) y el nivel funcional del bosque (FFL) como parte de este, ya que el 2003 DFL y FFL han quedado en desuso en Windows Server 2016 y ya no se admitirán en futuras versiones.

Para los clientes que necesiten más tiempo para evaluar mover su DFL & FFL desde 2003, la 2003 DFL y FFL seguirán compatibles con Windows 10 y Windows Server 2016 proporciona todos los controladores de dominio del dominio y el bosque están en Windows Server 2008, 2008R2, 2012, 2012R2, o 2016.

A Windows Server 2008 y más altos niveles funcionales de dominio, la replicación de servicio de archivos distribuido (DFS) se usa para replicar el contenido de la carpeta SYSVOL entre controladores de dominio. Si creas un nuevo dominio en el nivel funcional de dominio de Windows Server 2008 o una versión posterior, la replicación DFS automáticamente se usa para replicar SYSVOL. Si has creado el dominio de nivel inferior funcional, tendrás que migrar usen FRS para la replicación DFS para SYSVOL. Para ver los pasos de migración, puedes seguir la [procedimientos en TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o puede hacer referencia a la [mejorada conjunto de pasos en el blog de contenedor de archivos del equipo de almacenamiento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

Los niveles funcionales de bosque y dominio de Windows Server 2003 se siguen admitiendo, pero las organizaciones deben generar el nivel funcional a Windows Server 2008 (o superior si es posible) para garantizar la compatibilidad de replicación de SYSVOL y soporte técnico en el futuro. Además, hay muchas otras ventajas y las características disponibles en los niveles funcionales superior. Consulta los siguientes recursos para obtener más información:

## <a name="windows-server-2016"></a>Windows Server 2016

Admite el sistema operativo de controlador de dominio:

* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Características nivel funcionales del bosque Windows Server 2016

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2012R2 y las siguientes características están disponibles:
    * [Con privilegios de acceso management (PAM) con Microsoft Identity Manager (MIM)] (https://docs.microsoft.com/windows-server/identity/what's-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Características de nivel funcionales de dominio de Windows Server 2016

* Todas las características de Active Directory de manera predeterminada, todas las características de Windows Server 2012R2 nivel funcional del dominio, además de las siguientes características:
    * Controladores de dominio pueden admitir giro secretos NTLM públicos clave única de un usuario. 
    * Controladores de dominio pueden admitir la red que permitiría que NTLM cuando un usuario está restringida a determinados dispositivos Unidos a un dominio. 
    * Los clientes de Kerberos para autenticar correctamente con la extensión de actualidad PKInit obtendrán la identidad de clave pública SID desde cero.

    Para obtener más información, consulta [Novedades en la autenticación Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) y [Novedades de protección de credenciales](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Admite el sistema operativo de controlador de dominio:

* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Características de nivel funcionales de Windows Server 2012R2 bosque

* Todas las características que están disponibles en el equipo con Windows Server 2012 de bosque características funcionales de nivel, pero no adicionales.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Características de nivel funcionales de Windows Server 2012R2 dominio

* Todas las características de Active Directory de manera predeterminada, todas las características de nivel funcional del dominio de Windows Server 2012, además de las siguientes características:
    * Protecciones de lado del controlador de dominio para los usuarios protegido. La protección de los usuarios autenticarse en un dominio no puede de Windows Server 2012 R2:
        * Autenticar con la autenticación NTLM
        * Usar conjuntos de cifrado DES o RC4 de autenticación previa de Kerberos
        * Delegarse con la delegación restringida o sin restricciones
        * Renovación de vales (TGT) de usuario más allá de la duración inicial 4 horas
    * Directivas de autenticación
        * Bosque nuevo Active Directory directivas basadas que pueden aplicarse a cuentas en los dominios de Windows Server 2012 R2 para controlar qué hosts en una cuenta puede sesión desde y aplicar las condiciones de control de acceso para la autenticación a los servicios que se ejecutan con una cuenta.
    * Silos de directiva de autenticación
        * Basado en el bosque nuevo objeto de Active Directory, lo que puede crear una relación entre cuentas de usuario, un servicio administrado y equipo, que se usará para clasificar las cuentas de directivas de autenticación o de aislamiento de autenticación.

## <a name="windows-server-2012"></a>Windows Server 2012

Admite el sistema operativo de controlador de dominio:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Características nivel funcionales del bosque Windows Server 2012

* Todas las características que están disponibles en Windows Server 2008 R2 del bosque características funcionales de nivel, pero no adicionales.

### <a name="windows-server-2012-domain-functional-level-features"></a>Características de nivel funcionales de dominio de Windows Server 2012

* Todas las características de Active Directory de manera predeterminada, todas las características de Windows Server 2008R2 nivel funcional del dominio, además de las siguientes características:
    * La compatibilidad de KDC con notificaciones, autenticación compuesta y Kerberos armadura directiva de plantilla administrativa de KDC tiene dos opciones de configuración (siempre proporcionar notificaciones y producirá un error en las solicitudes de autenticación unarmored) que requieren el nivel funcional del dominio de Windows Server 2012. Para obtener más información, consulta [Novedades en la autenticación Kerberos](https://technet.microsoft.com/en-us/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Admite el sistema operativo de controlador de dominio:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Características de nivel funcionales de Windows Server 2008R2 bosque

* Todas las características que están disponibles en Windows Server 2003 bosque nivel funcional, además de las siguientes características:
    * Active Directory Papelera de reciclaje, que proporciona la capacidad de restaurar objetos eliminados en su totalidad mientras se ejecuta AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Características de nivel funcionales de Windows Server 2008R2 dominio

* Todas las características de Active Directory de manera predeterminada, todas las características de nivel funcional del dominio de Windows Server 2008, además de las siguientes características:
    * Comprobación del mecanismo de autenticación, que empaqueta la información sobre el tipo de método de inicio de sesión (tarjeta inteligente o nombre de usuario y contraseña) que se usa para autenticar a los usuarios de dominio dentro del token cada usuario Kerberos. Cuando esta característica está habilitada en un entorno de red que ha implementado una infraestructura de administración de identidad federada, como los servicios de federación de Active Directory (AD FS), la información en el token se puede extraer siempre que un usuario intenta acceder a cualquier aplicación para notificaciones que se ha desarrollado para determinar la autorización en función de método de inicio de sesión del usuario.
    * Administración automática de SPN para servicios que se ejecutan en un equipo en particular en el contexto de una cuenta de servicio administrada cuando el nombre DNS de host o nombre de los cambios de la cuenta de máquina. Para obtener más información sobre cuentas de servicio administradas, consulta [cuentas de servicio Step-by-Step guía](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Admite el sistema operativo de controlador de dominio:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Características de nivel funcionales de bosque de Windows Server 2008

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2003, pero no hay características adicionales están disponibles. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Características de nivel funcionales de dominio de Windows Server 2008

* Todos del predeterminado AD DS características, todas las características en el nivel funcional del dominio de Windows Server 2003, y están disponibles las siguientes características:
    * Soporte de replicación de sistema de archivos (DFS) distribuido para el volumen de sistema de Windows Server 2003 (SYSVOL)
        * Soporte de replicación de DFS proporciona replicación más sólida y detallada del contenido de SYSVOL.
        [!NOTE]>
        >A partir de Windows Server 2012 R2, servicio de replicación de archivos (FRS) está en desuso. Un nuevo dominio que se crea un controlador de dominio que se ejecuta al menos Windows Server 2012 R2 debe establecerse en el nivel funcional de dominio de Windows Server 2008 o superior.

    * Basado en dominio espacios de nombres DFS ejecutándose en modo de Windows Server 2008, que incluye soporte para aumentar la escalabilidad y la enumeración basada en el acceso. Además, basado en dominio espacios de nombres en el modo de Windows Server 2008 requieren el bosque de usar el nivel funcional del bosque de Windows Server 2003. Para obtener más información, consulta [elegir un tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
    * Advanced Encryption Standard (AES de 128 y AES 256) compatibilidad con el protocolo Kerberos. Para TGT para su emisión mediante AES, el nivel funcional del dominio debe ser Windows Server 2008 o una versión posterior y es necesario cambiar la contraseña del dominio. 
        * Para obtener más información, consulta [mejoras de Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).
        [!NOTE]>
        >Autenticación pueden producirse errores en un controlador de dominio una vez que se genera el nivel funcional del dominio a Windows Server 2008 o posterior si el controlador de dominio ya ha replicar el cambio DFL pero no ha actualizado la contraseña krbtgt todavía. En este caso, reiniciar el servicio KDC en el controlador de dominio se desencadenan una actualización en memoria de la nueva contraseña krbtgt y resolver errores de autenticación relacionados.

    * [El último inicio de sesión interactivo](https://go.microsoft.com/fwlink/?LinkId=180387) información muestra la siguiente información:
        * El número total de intentos de inicio de sesión erróneos en un servidor de Windows Server 2008 Unidos a un dominio o una estación de trabajo de Windows Vista
        * El número total de intentos no válidos después de iniciar sesión correctamente en un servidor de Windows Server 2008 o una estación de trabajo de Windows Vista
        * La hora del último intento de inicio de sesión erróneos en un equipo con Windows Server 2008 o una estación de trabajo de Windows Vista
        * Intenta la hora del último inicio de sesión correctamente en un servidor de Windows Server 2008 o una estación de trabajo de Windows Vista
    * Directivas de contraseña específicas hacen que sea posible para que especifique las directivas de bloqueo de cuenta y la contraseña para los usuarios y grupos de seguridad global en un dominio. Para obtener más información, consulta [guía paso a paso para la configuración de directiva de bloqueo de cuenta y contraseñas específicas](https://go.microsoft.com/fwlink/?LinkID=91477).
    * Escritorios virtuales personales
        * Para usar la funcionalidad agregada proporcionada por la pestaña de Escritorio Virtual Personal en el cuadro de diálogo de propiedades de la cuenta de usuario en equipos y usuarios de Active Directory, debe extender el esquema de AD DS para Windows Server 2008 R2 (versión del esquema de objeto = 47). Para obtener más información, consulta [implementación de escritorios virtuales personales mediante el uso de conexión de RemoteApp y escritorio Step-by-Step guía](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Admite el sistema operativo de controlador de dominio:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Características de nivel funcionales de bosque de Windows Server 2003

* Todas las características de AD DS de forma predeterminada y las siguientes características están disponibles:
    * Confianza de bosque
    * Cambiar el nombre de dominio
    * Replicación de valor vinculado
        - Replicación de valor vinculado hace posible para cambiar la pertenencia al grupo para almacenar y replicar los valores de miembros individuales en lugar de toda la pertenencia como una sola unidad de replicación. Almacenar y replicar los valores de miembros individuales usa menos ancho de banda de red y los ciclos de procesador menos durante la duplicación y evita que perder las actualizaciones al agregar o quitar a miembros varios al mismo tiempo en diferentes controladores de dominio.
    * La capacidad para implementar un controlador de dominio de solo lectura (RODC)
    * Algoritmos de Comprobador de coherencia de la información (KCC) y escalabilidad mejoradas
        - El generador de topología (ISTG) usa algoritmos mejorados que se escalan para admitir bosques con un número mayor de sitios de AD DS puede admitir el nivel funcional del bosque de Windows 2000. El algoritmo de elección de ISTG mejorado es un mecanismo menos intrusiva para elegir el ISTG en el nivel funcional del bosque de Windows 2000.
    * La capacidad para crear instancias de la clase auxiliar dinámica denominado **dynamicObject** en una partición de directorio de dominio
    * La capacidad para convertir un **inetOrgPerson** instancia del objeto en un **usuario** instancia de objeto y para completar la conversión en la dirección contraria
    * La capacidad para crear instancias de los nuevos tipos de grupo para admitir la autorización basadas en roles. 
        - Estos tipos se denominan grupos básicos de aplicación y los grupos de consulta LDAP.
    * Desactivación y la redefinición de atributos y las clases del esquema. Se pueden reutilizar los siguientes atributos: ldapDisplayName, schemaIdGuid, OID y mapiID.
    * Basado en dominio espacios de nombres DFS ejecutándose en modo de Windows Server 2008, que incluye soporte para aumentar la escalabilidad y la enumeración basada en el acceso. Para obtener más información, consulta [elegir un tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Características de nivel funcionales de dominio de Windows Server 2003

* Todas las características de AD DS de forma predeterminada, todas las características que están disponibles en el nivel funcional del dominio nativa de Windows 2000 y las siguientes características están disponibles:
    * La herramienta de administración de dominios, Netdom.exe, lo que permite cambiar el nombre de los controladores de dominio
    * Actualiza la marca de tiempo de inicio de sesión
        * La **lastLogonTimestamp** atributo se actualiza con la última vez de inicio de sesión del usuario o equipo. Este atributo se replica dentro del dominio.
    * La capacidad de establecer la **userPassword** atributo como la contraseña efectiva en **inetOrgPerson** y objetos de usuario
    * La capacidad para redirigir a los usuarios y equipos contenedores
        * De manera predeterminada, se proporcionan dos contenedores conocidos para alojamiento de cuentas de usuario y el equipo, es decir, cn = equipos,<domain root> y cn = usuarios,<domain root>. Esta característica permite que la definición de una nueva ubicación conocida para estas cuentas.
    * La capacidad para el Administrador de autorización para almacenar sus directivas de autorización en AD DS
    * Delegación restringida
        * La delegación restringida hace posible para aplicaciones sacar provecho de la delegación de credenciales de usuario segura mediante la autenticación Kerberos.
        * Puedes restringir delegación a solo los servicios de destino específicos.
    * Autenticación selectiva
        * Hace que la autenticación selectiva es posible especificar los usuarios y grupos de un bosque de confianza que se pueden autenticar en servidores de recursos en un bosque de confianza.

## <a name="windows-2000"></a>Windows 2000

Admite el sistema operativo de controlador de dominio:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Características de nivel funcionales de bosque nativo de Windows 2000

* Todas las características de predeterminados de AD DS están disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Características nivel funcionales del dominio nativo Windows 2000

* Todas las características de AD DS de forma predeterminada y las siguientes características de directorio son incluidos disponibles:
    * Grupos universales para grupos de distribución y de seguridad.
    * Anidamiento de grupos
    * Conversión de grupo, que permite la conversión entre grupos de seguridad y de distribución
    * Historial de identificador (SID) de seguridad

## <a name="next-steps"></a>Pasos siguientes

* [Aumentar el nivel funcional de dominio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentar el nivel funcional del bosque](https://technet.microsoft.com/library/cc730985.aspx)
