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
ms.openlocfilehash: c1e2108084b03fabbf7c6a18c2ecbcaf3cbd1dd9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868256"
---
# <a name="forest-and-domain-functional-levels"></a>Niveles funcionales de dominios y bosques

>Se aplica a: Windows Server

Los niveles funcionales determinan las capacidades disponibles de dominio o bosque de Active Directory Domain Services (AD DS). También determinan los sistemas operativos Windows Server que se pueden ejecutar en los controladores de dominio del dominio o bosque. Sin embargo, los niveles funcionales no afectan a los sistemas operativos que se pueden ejecutar en estaciones de trabajo y servidores miembro que se unen al dominio o bosque.

Al implementar AD DS, establezca los niveles funcionales de dominio y bosque en el valor más alto que pueda admitir el entorno. De este modo, puede usar tantas características AD DS como sea posible. Al implementar un nuevo bosque, se le pedirá que establezca el nivel funcional del bosque y, a continuación, establezca el nivel funcional del dominio. Puede establecer el nivel funcional del dominio en un valor superior al nivel funcional del bosque, pero no puede establecer el nivel funcional del dominio en un valor inferior al nivel funcional del bosque.

Con el fin de la vida de Windows 2003, los controladores de dominio (DC) de Windows 2003 deben actualizarse a Windows Server 2008, 2008R2, 2012, 2012R2, 2016 o 2019. Como resultado, todos los controladores de dominio que ejecuten Windows Server 2003 deben quitarse del dominio.

En los niveles funcionales de dominio de Windows Server 2008 y superior, se usa la replicación del servicio de archivos distribuido (DFS) para replicar el contenido de la carpeta SYSVOL entre controladores de dominio. Si crea un nuevo dominio en el nivel funcional del dominio de Windows Server 2008 o superior, se usa automáticamente Replicación DFS para replicar SYSVOL. Si ha creado el dominio en un nivel funcional inferior, tendrá que migrar desde el uso de FRS a la replicación DFS para SYSVOL. En el caso de los pasos de migración, puede seguir los [procedimientos de TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) o puede consultar el [conjunto de pasos simplificado en el blog del archivo. cab del equipo de almacenamiento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

No hay ningún nuevo nivel funcional de bosque o dominio agregado en esta versión.

El requisito mínimo para agregar un controlador de dominio de Windows Server 2019 es el nivel funcional de Windows Server 2008. El dominio también tiene que usar DFS-R como motor para replicar SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema operativo del controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Características del nivel funcional del bosque de Windows Server 2016

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2012R2 y las siguientes características están disponibles:
   * [Privileged Access Management (PAM) mediante Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2016

* Todas las características de Active Directory predeterminadas, todas las características del nivel funcional del dominio de Windows Server 2012R2, además de las siguientes características:
   * Los controladores de sesión pueden admitir el desplazamiento automático de NTLM y otros secretos basados en contraseña en una cuenta de usuario configurada para requerir la autenticación de PKI. Esta configuración también se conoce como "tarjeta inteligente requerida para el inicio de sesión interactivo".
   * Los controladores de dominio pueden admitir la red NTLM cuando un usuario está restringido a determinados dispositivos Unidos a un dominio.
   * Los clientes Kerberos que se autentican correctamente con la extensión de actualización PKInit obtendrán el SID de identidad de clave pública nuevo.

    Para obtener más información, vea [novedades de la autenticación Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) y [novedades de la protección de credenciales](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection) .

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Sistema operativo del controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Características del nivel funcional del bosque de Windows Server 2012R2

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2012, pero sin características adicionales.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2012R2

* Todas las características de Active Directory predeterminadas, todas las características del nivel funcional del dominio de Windows Server 2012, además de las siguientes características:
   * Protecciones del controlador de dominio para usuarios protegidos. Los usuarios protegidos que se autentiquen en un dominio de Windows Server 2012 R2 ya no podrán:
      * Autenticarse con autenticación NTLM
      * Uso de conjuntos de cifrado DES o RC4 en la autenticación previa de Kerberos
      * Delegados con delegación restringida o sin restricciones
      * Renovar los vales de usuario (TGT) más allá de la duración inicial de 4 horas
   * Authentication Policies
      * Nuevas directivas de Active Directory basadas en bosques que se pueden aplicar a las cuentas de dominios de Windows Server 2012 R2 para controlar a qué hosts puede iniciar sesión una cuenta y aplicar condiciones de control de acceso para la autenticación en los servicios que se ejecutan como una cuenta.
   * Authentication Policy Silos
      * Nuevo objeto de Active Directory basado en bosque, que puede crear una relación entre el usuario, el servicio administrado y el equipo, las cuentas que se van a usar para clasificar las cuentas de las directivas de autenticación o el aislamiento de autenticación.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema operativo del controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Características del nivel funcional del bosque de Windows Server 2012

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2008 R2, pero sin características adicionales.

### <a name="windows-server-2012-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2012

* Todas las características de Active Directory predeterminadas, todas las características del nivel funcional del dominio de Windows Server 2008R2, además de las siguientes características:
   * La compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos de la Directiva de plantillas administrativas de KDC tiene dos configuraciones (proporcionar siempre notificaciones y generar un error en las solicitudes de autenticación sin protección) que requieren el nivel funcional de dominio de Windows Server 2012. Para obtener más información, vea [novedades de la autenticación Kerberos](https://technet.microsoft.com/library/hh831747.aspx) .

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistema operativo del controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Características del nivel funcional del bosque de Windows Server 2008R2

* Todas las características disponibles en el nivel funcional del bosque de Windows Server 2003, más las siguientes características:
   * Papelera de reciclaje de Active Directory, que proporciona la capacidad de restaurar completamente objetos eliminados mientras se ejecuta AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2008R2

* Todas las características de Active Directory predeterminadas, todas las características del nivel funcional del dominio de Windows Server 2008, además de las siguientes características:
   * La garantía del mecanismo de autenticación, que empaqueta la información sobre el tipo de método de inicio de sesión (tarjeta inteligente o nombre de usuario/contraseña) que se usa para autenticar a los usuarios del dominio dentro del token de Kerberos de cada usuario. Cuando esta característica está habilitada en un entorno de red que ha implementado una infraestructura de administración de identidades federada, como Servicios de federación de Active Directory (AD FS) (AD FS), se puede extraer la información del token cada vez que un usuario intente acceder a cualquier aplicación compatible con notificaciones que se ha desarrollado para determinar la autorización basada en el método de inicio de sesión de un usuario.
   * Administración automática de SPN para servicios que se ejecutan en un equipo determinado en el contexto de una cuenta de servicio administrada cuando el nombre o el nombre de host DNS de la cuenta de equipo cambia. Para obtener más información sobre las cuentas de servicio administradas, vea [la guía paso a paso de las cuentas de servicio](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema operativo del controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Características del nivel funcional del bosque de Windows Server 2008

* Todas las características que están disponibles en el nivel funcional del bosque de Windows Server 2003, pero no hay características adicionales disponibles. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2008

* Todas las características predeterminadas de AD DS, todas las características del nivel funcional del dominio de Windows Server 2003 y las siguientes características están disponibles:
  * Compatibilidad de la replicación de Sistema de archivos distribuido (DFS) con el volumen del sistema de Windows Server 2003 (SYSVOL)
    * La compatibilidad con la replicación DFS proporciona una replicación más sólida y detallada del contenido de SYSVOL.

      > [!NOTE]
      > A partir de Windows Server 2012 R2, el servicio de replicación de archivos (FRS) está en desuso. Un dominio nuevo que se crea en un controlador de dominio que ejecute como mínimo Windows Server 2012 R2 debe establecerse en el nivel funcional del dominio de Windows Server 2008 o superior.

  * Espacios de nombres DFS basados en dominio que se ejecutan en modo Windows Server 2008, que incluye compatibilidad con la enumeración basada en el acceso y una mayor escalabilidad. Los espacios de nombres basados en dominio en modo Windows Server 2008 también requieren que el bosque use el nivel funcional del bosque de Windows Server 2003. Para obtener más información, vea [elegir un tipo de espacio de nombres](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Estándar de cifrado avanzado (AES 128 y AES 256) para el protocolo Kerberos. Para que se emitan TGT mediante AES, el nivel funcional del dominio debe ser Windows Server 2008 o superior y debe cambiarse la contraseña del dominio. 
    * Para obtener más información, vea [mejoras de Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Los errores de autenticación pueden producirse en un controlador de dominio después de que el nivel funcional del dominio se eleve a Windows Server 2008 o superior si el controlador de dominio ya ha replicado el cambio de nivel funcional pero aún no ha actualizado la contraseña de krbtgt. En este caso, si se reinicia el servicio KDC en el controlador de dominio, se desencadenará una actualización en memoria de la nueva contraseña de krbtgt y se resolverán los errores de autenticación relacionados.

  * [Último inicio de sesión interactivo](https://go.microsoft.com/fwlink/?LinkId=180387) Información muestra la siguiente información:
     * El número total de intentos de inicio de sesión erróneos en un servidor Windows Server 2008 o una estación de trabajo de Windows Vista Unidos a un dominio
     * El número total de intentos de inicio de sesión erróneos después de un inicio de sesión correcto en un servidor Windows Server 2008 o una estación de trabajo de Windows Vista
     * La hora del último intento de inicio de sesión incorrecto en Windows Server 2008 o una estación de trabajo de Windows Vista
     * La hora del último intento de inicio de sesión correcto en un servidor de Windows Server 2008 o una estación de trabajo de Windows Vista
  * Las directivas de contraseña específica permiten especificar directivas de bloqueo de cuentas y contraseñas para usuarios y grupos de seguridad global en un dominio. Para obtener más información, consulte la [Guía paso a paso para la configuración de directivas de bloqueo de cuentas y contraseñas específicas](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Escritorios virtuales personales
     * Para usar la funcionalidad agregada proporcionada por la pestaña escritorio virtual personal del cuadro de diálogo Propiedades de cuenta de usuario de Active Directory usuarios y equipos, el esquema de AD DS debe ampliarse para Windows Server 2008 R2 (versión de objeto de esquema = 47). Para obtener más información, consulte [implementación de escritorios virtuales personales mediante conexión de RemoteApp y escritorio guía paso a paso](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema operativo del controlador de dominio admitido:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Características del nivel funcional del bosque de Windows Server 2003

* Todas las características de AD DS predeterminadas y las siguientes características están disponibles:
   * Confianza de bosque
   * Cambio de nombre de dominio
   * Replicación de valor vinculado
      - La replicación de valor vinculado permite cambiar la pertenencia a grupos para almacenar y replicar valores para miembros individuales en lugar de replicar toda la pertenencia como una sola unidad. El almacenamiento y la replicación de los valores de miembros individuales utiliza menos ancho de banda de red y menos ciclos de procesador durante la replicación, y evita que se pierdan actualizaciones cuando se agregan o quitan varios miembros simultáneamente en diferentes controladores de dominio.
   * La capacidad de implementar un controlador de dominio de solo lectura (RODC)
   * Algoritmos y escalabilidad mejorados del comprobador de coherencia de la información (KCC)
      - El generador de topología entre sitios (ISTG) usa algoritmos mejorados que se escalan para admitir bosques con un número mayor de sitios que AD DS pueden admitir en el nivel funcional del bosque de Windows 2000. El algoritmo de elección de ISTG mejorado es un mecanismo menos intrusivo para elegir el ISTG en el nivel funcional del bosque de Windows 2000.
   * La capacidad de crear instancias de la clase auxiliar dinámica denominada **dynamicObject** en una partición de directorio de dominio
   * La capacidad de convertir una instancia de objeto **inetOrgPerson** en una instancia de objeto de **usuario** y de completar la conversión en la dirección opuesta.
   * La capacidad de crear instancias de nuevos tipos de grupo para admitir la autorización basada en roles. 
      - Estos tipos se denominan grupos básicos de aplicación y grupos de consulta LDAP.
   * Desactivación y nueva definición de atributos y clases en el esquema. Se pueden reutilizar los siguientes atributos: ldapDisplayName, schemaIdGuid, OID y mapiID.
   * Espacios de nombres DFS basados en dominio que se ejecutan en modo Windows Server 2008, que incluye compatibilidad con la enumeración basada en el acceso y una mayor escalabilidad. Para obtener más información, vea [elegir un tipo de espacio de nombres](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Características de nivel funcional de dominio de Windows Server 2003

* Todas las características de AD DS predeterminadas, todas las características que están disponibles en el nivel funcional de dominio nativo de Windows 2000 y las siguientes características están disponibles:
   * La herramienta de administración de dominios, Netdom. exe, que permite cambiar el nombre de los controladores de dominio.
   * Actualizaciones de marca de tiempo de inicio de sesión
      * El atributo **lastLogonTimestamp** se actualiza con la hora en que el usuario o equipo inició sesión por última vez. Este atributo se replica dentro del dominio.
   * La capacidad de establecer el atributo **userPassword** como la contraseña efectiva en objetos **inetOrgPerson** y User
   * La capacidad de redirigir los contenedores de usuarios y equipos
      * De forma predeterminada, se proporcionan dos contenedores conocidos para albergar cuentas de equipo y de usuario, es decir, CN =<domain root> Computers, y CN<domain root>= users,. Esta característica permite definir una ubicación nueva y conocida para estas cuentas.
   * La capacidad de administrador de autorización para almacenar sus directivas de autorización en AD DS
   * Delegación restringida
      * La delegación restringida permite que las aplicaciones aprovechen la delegación segura de credenciales de usuario por medio de la autenticación basada en Kerberos.
      * Solo puede restringir la delegación a servicios de destino específicos.
   * Autenticación selectiva
      * La autenticación selectiva permite especificar los usuarios y grupos de un bosque de confianza que tienen permiso para autenticarse en servidores de recursos en un bosque que confía.

## <a name="windows-2000"></a>Windows 2000

Sistema operativo del controlador de dominio admitido:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Características del nivel funcional del bosque nativo de Windows 2000

* Todas las características de AD DS predeterminadas están disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Características de nivel funcional de dominio nativo de Windows 2000

* Todas las características de AD DS predeterminadas y las siguientes características de directorio están disponibles, entre las que se incluyen:
   * Grupos universales para los grupos de distribución y de seguridad.
   * Anidación de grupos
   * Conversión de grupos, que permite la conversión entre grupos de seguridad y distribución
   * Historial de identificadores de seguridad (SID)

## <a name="next-steps"></a>Pasos siguientes

* [Elevar el nivel funcional del dominio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Elevar el nivel funcional del bosque](https://technet.microsoft.com/library/cc730985.aspx)
