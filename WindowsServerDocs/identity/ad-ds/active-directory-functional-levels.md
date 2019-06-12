---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Niveles funcionales de Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: cb9b5b9448f364760c3d2a7e43edd01a5a9f7f9d
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719664"
---
# <a name="forest-and-domain-functional-levels"></a>Niveles funcionales de bosque y dominio

>Se aplica a: Windows Server

Los niveles funcionales determinan las funcionalidades disponibles de dominio o bosque de Active Directory Domain Services (AD DS). También determinan qué sistemas operativos de Windows Server puede ejecutar en controladores de dominio en el dominio o bosque. Sin embargo, los niveles funcionales no afectan a qué sistemas operativos se pueden ejecutar en las estaciones de trabajo y los servidores miembro que están unidos al dominio o bosque.

Al implementar AD DS, Establece los niveles funcionales de dominio y bosque en el valor más alto que admita su entorno. De este modo, puede usar características de AD DS tantos como sea posible. Al implementar un nuevo bosque, deberá establecer el nivel funcional del bosque y, a continuación, establezca el nivel funcional del dominio. Puede establecer el nivel funcional del dominio en un valor que es mayor que el nivel funcional del bosque, pero no se puede establecer el nivel funcional del dominio en un valor que es menor que el nivel funcional del bosque.

Con el final del ciclo de vida de Windows 2003, controladores de dominio (DC) de Windows 2003 deben actualizarse a Windows Server 2008, 2008R2, 2012, 2012 R2, 2016 o 2019. Como resultado, cualquier controlador de dominio que ejecuta Windows Server 2003 debe quitarse del dominio.

En el Windows Server 2008 y los niveles funcionales de dominio más altos, replicación de servicio de archivos distribuido (DFS) se usa para replicar el contenido de la carpeta SYSVOL entre controladores de dominio. Si crea un nuevo dominio en el nivel funcional del dominio de Windows Server 2008 o posterior, se usa automáticamente la replicación DFS para replicar SYSVOL. Si ha creado el dominio en un nivel funcional inferior, deberá migrar del uso de FRS a replicación DFS para SYSVOL. Para conocer los pasos de migración, puede seguir el [procedimientos en TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o puede hacer referencia a la [simplificado de conjunto de pasos en el blog de contenedor de archivos del equipo de almacenamiento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

No hay ningún nuevo bosque o los niveles funcionales de dominio agregados en esta versión.

El requisito mínimo para agregar un controlador de dominio de Windows Server 2019 es un nivel funcional de Windows Server 2008. El dominio también tiene que usar DFS-R como el motor para replicar SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema de operativo del controlador de dominio admitidos:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Características de nivel funcional de bosque de Windows Server 2016

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2012 R2 y las siguientes características están disponibles:
   * [Administración de acceso con privilegios (PAM) mediante Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2016

* Todas las características predeterminadas de Active Directory, todas las características desde el nivel funcional del dominio de Windows Server 2012 R2, además de las siguientes características:
   * Los controladores de dominio pueden admitir graduales automáticas de NTLM y otros secretos basada en contraseña en una cuenta de usuario configurado para requerir la autenticación PKI. Esta configuración también es conocido como "Tarjeta inteligente es necesaria para el inicio de sesión interactivo"
   * Los controladores de dominio pueden admitir la red permitiendo NTLM cuando un usuario está restringido a determinados dispositivos Unidos a dominio.
   * Los clientes de Kerberos para autenticar correctamente con la extensión de la actualización de PKInit obtendrá la identidad de clave pública nueva SID.

    Para obtener más información, consulte [What ' s New in Kerberos Authentication](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) y [cuáles son las novedades en protección de credenciales](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Sistema de operativo del controlador de dominio admitidos:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Características de nivel funcionales de Windows Server 2012 R2 bosque

* Todas las características que están disponibles en Windows Server 2012 funcionales nivel, pero no características adicionales del bosque.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Características de nivel funcionales de Windows Server 2012R2 dominio

* Todas las características predeterminadas de Active Directory, todas las características desde el nivel funcional del dominio de Windows Server 2012, además de las siguientes características:
   * Protecciones de lado del controlador de dominio para los usuarios protegidos. La protección de autenticación de usuarios en un dominio ya no puede de Windows Server 2012 R2:
      * Autenticar con la autenticación NTLM
      * Usar conjuntos de cifrado DES o RC4 en la autenticación Kerberos previa
      * Se puede delegar con delegación limitada o no
      * Renovación de vales de usuario (TGT) más allá de la duración inicial de 4 horas
   * Authentication Policies
      * Directivas de Active Directory basado en un bosque nuevo que se pueden aplicar a cuentas en dominios de Windows Server 2012 R2 para controlar qué hosts una cuenta puede inicio de sesión de y se aplican las condiciones de control de acceso para la autenticación a servicios que se ejecutan como una cuenta.
   * Authentication Policy Silos
      * Basado en un bosque nuevo objeto de Active Directory, que puede crear una relación entre cuentas de usuario, un servicio administrado y equipo, que se utilizará para clasificar las cuentas para las directivas de autenticación o para el aislamiento de la autenticación.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema de operativo del controlador de dominio admitidos:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Características de nivel funcional de bosque de Windows Server 2012

* Todas las características que están disponibles en Windows Server 2008 R2 del bosque funcionales nivel, pero no características adicionales.

### <a name="windows-server-2012-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2012

* Todas las características predeterminadas de Active Directory, todas las características desde el nivel funcional del dominio de Windows Server 2008 R2, además de las siguientes características:
   * La compatibilidad del KDC con notificaciones, autenticación compuesta y protección directiva de plantillas administrativas KDC tiene dos configuraciones (siempre proporcionar notificaciones y producirá un error en solicitudes de autenticación) que requieren el nivel funcional del dominio de Windows Server 2012 de Kerberos. Para obtener más información, consulte [What ' s New in Kerberos Authentication](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistema de operativo del controlador de dominio admitidos:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Características de nivel funcionales de Windows Server 2008 R2 bosque

* Todas las características disponibles en el nivel funcional del bosque de Windows Server 2003, más las siguientes características:
   * Papelera de reciclaje de Active Directory, que proporciona la capacidad de restaurar completamente objetos eliminados mientras se ejecuta AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Características de nivel funcionales de Windows Server 2008 R2 dominio

* Todas las características predeterminadas de Active Directory, todas las características desde el nivel funcional del dominio de Windows Server 2008, además de las siguientes características:
   * La comprobación del mecanismo de autenticación, que empaqueta la información sobre el tipo de método de inicio de sesión (tarjeta inteligente o nombre de usuario/contraseña) empleado para autenticar a usuarios del dominio dentro del token de Kerberos de cada usuario. Cuando esta característica está habilitada en un entorno de red que ha implementado una infraestructura de administración de identidades federadas, como servicios de federación de Active Directory (AD FS), la información del token, a continuación, se puede extraer siempre que un usuario intenta acceder a ninguno aplicación para notificaciones que se ha desarrollado para determinar la autorización basada en el método de inicio de sesión del usuario.
   * Administración automática de SPN para servicios que se ejecutan en un equipo determinado en el contexto de una cuenta de servicio administrada cuando el nombre DNS de host o nombre de los cambios de la cuenta de equipo. Para obtener más información sobre las cuentas de servicio administradas, vea [guía paso a paso de las cuentas de servicio](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema de operativo del controlador de dominio admitidos:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Características de nivel funcional de bosque de Windows Server 2008

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2003, pero no características adicionales están disponibles. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2008

* Todos los de la predeterminada de AD DS características, todas las características desde el nivel funcional del dominio de Windows Server 2003, y están disponibles las siguientes características:
  * Compatibilidad con la replicación de Distributed File System (DFS) para el volumen de sistema de Windows Server 2003 (SYSVOL)
    * Compatibilidad con la replicación DFS proporciona una replicación más sólida y detallada del contenido de SYSVOL.

      > [!NOTE]
      > A partir de Windows Server 2012 R2, servicio de replicación de archivos (FRS) está en desuso. Un dominio nuevo que se crea en un controlador de dominio que ejecute al menos Windows Server 2012 R2 debe establecerse en el nivel funcional del dominio de Windows Server 2008 o posterior.

  * Basado en dominio espacios de nombres DFS que se ejecuta en modo de Windows Server 2008, que incluye soporte técnico para aumentar la escalabilidad y el acceso según la enumeración. Espacios de nombres basados en dominio en modo Windows Server 2008 también requieren el bosque que desea utilizar el nivel funcional del bosque de Windows Server 2003. Para obtener más información, consulte [elegir un tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Advanced Encryption Standard (AES 128 y AES 256) compatibilidad con el protocolo Kerberos. En el orden de los TGT que se emitan mediante AES, el nivel funcional del dominio debe ser Windows Server 2008 o posterior y debe cambiarse la contraseña de dominio. 
    * Para obtener más información, consulte [mejoras de Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Errores de autenticación pueden producirse en un controlador de dominio después de eleva el nivel funcional del dominio a Windows Server 2008 o posterior si el controlador de dominio ya ha replicado el cambio del nivel funcional de dominio, pero no ha actualizado la contraseña de krbtgt todavía. En este caso, un reinicio del servicio KDC en el controlador de dominio desencadenará una actualización en la memoria de la nueva contraseña de krbtgt y resolver errores de autenticación relacionadas.

  * [Último inicio de sesión interactivo](https://go.microsoft.com/fwlink/?LinkId=180387) información muestra la siguiente información:
     * El número total de intentos de inicio de sesión en un servidor unido a un dominio de Windows Server 2008 o una estación de trabajo de Windows Vista
     * El número total de intentos de inicio de sesión después de iniciar sesión correctamente en un servidor de Windows Server 2008 o una estación de trabajo de Windows Vista
     * La hora del último intento de inicio de sesión en un equipo con Windows Server 2008 o una estación de trabajo de Windows Vista
     * Intento de la hora del último inicio de sesión correcto en un servidor de Windows Server 2008 o una estación de trabajo de Windows Vista
  * Directivas de contraseña específica permiten especificar directivas de bloqueo de cuentas y contraseñas para usuarios y grupos de seguridad global en un dominio. Para obtener más información, consulte [guía paso a paso para la configuración de directiva de bloqueo de cuenta y contraseña específica](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Escritorios virtuales personales
     * Para usar la funcionalidad agregada proporcionada por la ficha de Escritorio Virtual Personal en el cuadro de diálogo Propiedades de la cuenta de usuario en usuarios y equipos de usuarios de Active Directory, se debe extender el esquema de AD DS para Windows Server 2008 R2 (versión de esquema de objeto = 47). Para obtener más información, consulte [implementar escritorios virtuales personales usando RemoteApp y Guía paso a paso de conexión de escritorio](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema de operativo del controlador de dominio admitidos:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Características de nivel funcional de bosque de Windows Server 2003

* Todas las características predeterminadas de AD DS y las siguientes características están disponibles:
   * Confianza de bosque
   * Cambio de nombre de dominio
   * Replicación de valor vinculado
      - Replicación de valor vinculado permite cambiar la pertenencia a grupo para almacenar y replicar los valores para miembros individuales en lugar de replicar toda la pertenencia como una sola unidad. Almacenar y replicar los valores de los miembros individuales usa menos ancho de banda de red y menos ciclos de procesador durante la replicación y evita perder actualizaciones cuando se agregan o quitan a varios miembros simultáneamente en diferentes controladores de dominio.
   * La capacidad de implementar un controlador de dominio de solo lectura (RODC)
   * Los algoritmos del Comprobador de coherencia de la información (KCC) y la escalabilidad mejorada
      - El generador de topología entre sitios (ISTG) usa algoritmos mejorados que se escalan para admitir bosques con un número mayor de sitios de AD DS puede admitir en el nivel funcional del bosque de Windows 2000. El algoritmo mejorado de elección de ISTG es un mecanismo menos intrusivo para elegir el ISTG en el nivel funcional del bosque de Windows 2000.
   * La capacidad de crear instancias de la clase auxiliar dinámica llamada **dynamicObject** en una partición de directorio de dominio
   * La capacidad de convertir un **inetOrgPerson** instancia del objeto en un **usuario** instancia de objeto y para completar la conversión en la dirección opuesta
   * La capacidad de crear instancias de tipos de grupo nuevas para admitir la autorización basada en roles. 
      - Estos tipos se denominan grupos básicos de aplicación y los grupos de consulta LDAP.
   * Desactivación y nueva definición de atributos y clases en el esquema. Se pueden reutilizar los siguientes atributos: ldapDisplayName, schemaIdGuid, OID y mapiID.
   * Basado en dominio espacios de nombres DFS que se ejecuta en modo de Windows Server 2008, que incluye soporte técnico para aumentar la escalabilidad y el acceso según la enumeración. Para obtener más información, consulte [elegir un tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2003

* Todas las características de AD DS de forma predeterminada, todas las características que están disponibles en el nivel funcional del dominio en modo nativo de Windows 2000 y las características siguientes están disponibles:
   * La herramienta de administración de dominios, Netdom.exe, lo que permite cambiar el nombre de los controladores de dominio
   * Actualiza la marca de tiempo de inicio de sesión
      * El atributo **lastLogonTimestamp** se actualiza con la hora en que el usuario o equipo inició sesión por última vez. Este atributo se replica dentro del dominio.
   * La capacidad de establecer el **userPassword** atributo como la contraseña efectiva en **inetOrgPerson** y objetos de usuario
   * La capacidad de redirigir los usuarios y equipos de contenedores
      * De forma predeterminada, se proporcionan dos contenedores conocidos para la caja de cuentas de usuario y equipo, a saber, cn = Computers,<domain root> y cn = Users,<domain root>. Esta característica permite la definición de una ubicación nueva conocida para estas cuentas.
   * La capacidad para el Administrador de autorización para almacenar sus directivas de autorización en AD DS
   * Delegación restringida
      * La delegación restringida hace posible para que las aplicaciones aprovechen la delegación segura de credenciales de usuario por medio de autenticación basada en Kerberos.
      * Puede restringir la delegación en servicios de destino específicas solo.
   * Autenticación selectiva
      * Autenticación selectiva hace posible especificar los usuarios y grupos de un bosque de confianza que se les permite autenticarse en servidores de recursos en un bosque que confía.

## <a name="windows-2000"></a>Windows 2000

Sistema de operativo del controlador de dominio admitidos:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Características de nivel funcional de bosque nativo de Windows 2000

* Todas las características de valor predeterminado de AD DS están disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Características de nivel funcional de dominio en modo nativo de Windows 2000

* Todas las características predeterminadas de AD DS y las siguientes características de directorio son incluidos disponibles:
   * Grupos universales para grupos de distribución y seguridad.
   * Anidamiento de grupos
   * Conversión de grupos, lo que permite la conversión entre grupos de seguridad y distribución
   * Historial de seguridad (SID) de identificador

## <a name="next-steps"></a>Pasos siguientes

* [Elevar el nivel funcional de dominio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Elevar el nivel funcional de bosque](https://technet.microsoft.com/library/cc730985.aspx)
