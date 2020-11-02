---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Niveles funcionales de Windows Server 2016
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/25/2020
ms.topic: article
ms.custom: it-pro
ms.reviewer: maheshu
ms.openlocfilehash: b74bb786b3d1a6ec8a1f96054b2d74ca93bd9bcf
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068379"
---
# <a name="forest-and-domain-functional-levels"></a>Niveles funcionales de bosque y dominio

>Se aplica a: Windows Server

Los niveles funcionales determinan las funcionalidades disponibles de dominio o bosque de Active Directory Domain Services (AD DS). También determinan los sistemas operativos Windows Server que se pueden ejecutar en los controladores de dominio del dominio o del bosque. Sin embargo, los niveles funcionales no afectan a los sistemas operativos que se pueden ejecutar en las estaciones de trabajo y los servidores miembro que están unidos al dominio o al bosque.

Cuando implementes AD DS, establece los niveles funcionales de dominio y bosque en el valor más alto que admite el entorno. De esta manera, podrás usar el mayor número posible de características de AD DS. Al implementar un bosque nuevo, se te pide que establezcas el nivel funcional del bosque y, después, el nivel funcional del dominio. Puedes establecer el nivel funcional del dominio en un valor que sea superior al nivel funcional del bosque, pero no puedes establecerlo en un valor que sea inferior al nivel funcional del bosque.

Con el fin del ciclo de vida de Windows Server 2003, 2008 y 2008 R2, los controladores de dominio (DC) deben actualizarse a Windows Server 2012, 2012 R2, 2016 o 2019. Como resultado, todos los controladores de dominio que ejecuten Windows Server 2008 R2 y anteriores deben quitarse del dominio.

En los niveles funcionales de dominio de Windows Server 2008 y superior, se usa la Replicación del sistema de archivos distribuido (DFS) para replicar el contenido de la carpeta SYSVOL entre controladores de dominio. Si creas un nuevo dominio en el nivel funcional del dominio de Windows Server 2008 o superior, se usa automáticamente Replicación DFS para replicar SYSVOL. Si has creado el dominio en un nivel funcional inferior, tendrás que migrar del uso de FRS a la replicación DFS para SYSVOL. En el caso de los pasos de migración, puedes seguir los [procedimientos de TechNet](../../storage/dfs-replication/migrate-sysvol-to-dfsr.md) o puedes consultar el [conjunto de pasos simplificado en el blog de contenedor de archivos del equipo de almacenamiento](https://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx). Windows Server 2016 RS1 es la última versión de Windows Server que incluye FRS.

## <a name="windows-server-2019"></a>Windows Server 2019

No se ha agregado ningún nuevo nivel funcional de bosque o dominio en esta versión.

El requisito mínimo para agregar un controlador de dominio de Windows Server 2019 es el nivel funcional de Windows Server 2008. El dominio también tiene que usar DFS-R como motor para replicar SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema operativo de controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Características del nivel funcional de bosque de Windows Server 2016

* Todas las características disponibles en el nivel funcional del bosque de Windows Server 2012R2, además las siguientes características. están disponibles:
   * [Privileged Access Management (PAM) con Microsoft Identity Manager (MIM)](../whats-new-active-directory-domain-services.md#privileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Características del nivel funcional de dominio de Windows Server 2016

* Todas las características predeterminadas de Active Directory, todas las características del nivel funcional del dominio de Windows Server 2012R2 y las características siguientes:
   * Los controladores de dominio pueden admitir la implementación automática de NTLM y otros secretos basados en contraseña en una cuenta de usuario configurada para requerir la autenticación de PKI. Esta configuración también se conoce como "tarjeta inteligente requerida para el inicio de sesión interactivo".
   * Los controladores de dominio pueden admitir NTLM de red cuando un usuario está restringido a determinados dispositivos unidos a un dominio.
   * Los clientes Kerberos que se autentican correctamente con la extensión de actualización PKInit obtendrán el SID de identidad de clave pública actualizado.

    Para más información, consulta [Novedades de la autenticación Kerberos](../../security/kerberos/whats-new-in-kerberos-authentication.md) y [Novedades en la protección de credenciales](../../security/credentials-protection-and-management/whats-new-in-credential-protection.md)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Sistema operativo de controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Características del nivel funcional de bosque de Windows Server 2012R2

* Todas las características disponibles en el nivel funcional del bosque de Windows Server 2012, pero no características adicionales.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Características del nivel funcional de dominio de Windows Server 2012R2

* Todas las características predeterminadas de Active Directory, todas las características del nivel funcional del dominio de Windows Server 2012 y las características siguientes:
   * Protecciones en el lado del controlador de dominio para los usuarios protegidos. Los usuarios protegidos que se autentiquen en un dominio de Windows Server 2012 R2 ya no podrán:
      * Autenticarse mediante la autenticación NTLM
      * Usar los conjuntos de cifrado DES o RC4 en la autenticación previa de Kerberos
      * Delegarse mediante una delegación con o sin restricciones
      * Renovar los vales de usuario (TGT) más allá de las 4 horas de vigencia inicial
   * Authentication Policies
      * Nuevas directivas de Active Directory basadas en bosques que se pueden aplicar a las cuentas en los dominios de Windows Server 2012 R2 para controlar en qué host puede iniciar sesión una cuenta y aplicar condiciones de control de acceso para la autenticación en los servicios que se ejecutan como una cuenta.
   * Authentication Policy Silos
      * Nuevo objeto de Active Directory basado en bosque, que puede crear una relación entre el usuario, el servicio administrado y el equipo, las cuentas que se van a usar para clasificar las cuentas de las directivas de autenticación o el aislamiento de autenticación.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema operativo de controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Características del nivel funcional de bosque de Windows Server 2012

* Todas las características disponibles en el nivel funcional del bosque de Windows Server 2008 R2, pero no características adicionales.

### <a name="windows-server-2012-domain-functional-level-features"></a>Características del nivel funcional de dominio de Windows Server 2012

* Todas las características predeterminadas de Active Directory, todas las características del nivel funcional del dominio de Windows Server 2008R2 y las características siguientes:
   * La directiva de plantilla administrativa de compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos tiene dos configuraciones (proporcionar siempre notificaciones y error de solicitudes de autenticación sin blindar) que requieren el nivel funcional de dominio de Windows Server 2012. Para más información, consulta [Novedades de la autenticación Kerberos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)).

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistema operativo de controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Características del nivel funcional de bosque de Windows Server 2008R2

* Todas las características disponibles en el nivel funcional del bosque de Windows Server 2003, más las siguientes características:
   * Papelera de reciclaje de Active Directory, que proporciona la capacidad de restaurar completamente objetos eliminados mientras se ejecuta AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Características del nivel funcional de dominio de Windows Server 2008R2

* Todas las características predeterminadas de Active Directory, todas las características del nivel funcional del dominio de Windows Server 2008 y las características siguientes:
   * La comprobación del mecanismo de autenticación, que empaqueta la información sobre el tipo de método de inicio de sesión (tarjeta inteligente o nombre de usuario/contraseña) empleado para autenticar a usuarios del dominio dentro del token de Kerberos de cada usuario. Si esta característica está habilitada en un entorno de red que ha implementado una infraestructura de administración de identidades federadas, como Servicios de federación de Active Directory (AD FS), la información del token se puede extraer siempre que un usuario intente obtener acceso a cualquier aplicación para notificaciones que se haya desarrollado para determinar la autorización en función del método de inicio de sesión de un usuario.
   * Administración automática de SPN para servicios que se ejecutan en un equipo en particular en el contexto de una cuenta de servicio administrada cuando se modifica el nombre o el nombre de host DNS de la cuenta del equipo. Para obtener más información sobre cuentas de servicio administradas, consulta la [Guía paso a paso de las cuentas de servicio](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema operativo de controlador de dominio admitido:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Características del nivel funcional de bosque de Windows Server 2008

* Están disponibles todas las características disponibles en el nivel funcional del bosque de Windows Server 2003, pero no características adicionales.

### <a name="windows-server-2008-domain-functional-level-features"></a>Características del nivel funcional de dominio de Windows Server 2008

* Todas las características predeterminadas de AD DS, todas las características del nivel funcional del dominio de Windows Server 2003 y las características siguientes están disponibles:
  * Compatibilidad de la replicación del Sistema de archivos distribuido (DFS) con el volumen del sistema de Windows Server 2003 (SYSVOL)
    * La compatibilidad con la replicación DFS proporciona una replicación más sólida y detallada del contenido de SYSVOL.

      > [!NOTE]
      > A partir de Windows Server 2012 R2, el servicio de replicación de archivos (FRS) está en desuso. Un dominio nuevo que se crea en un controlador de dominio que ejecuta como mínimo Windows Server 2012 R2 debe establecerse en el nivel funcional del dominio de Windows Server 2008 o superior.

  * Los espacios de nombres de DFS basados en dominio que se ejecutan en modo Windows Server 2008, que incluyen compatibilidad con la enumeración basada en el acceso y mayor escalabilidad. Los espacios de nombres basados en dominio en modo Windows Server 2008 también requieren que el bosque use el nivel funcional del bosque de Windows Server 2003. Para obtener más información, consulta [Elegir un tipo de espacio de nombres](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Compatibilidad del Estándar de cifrado avanzado (AES 128 y 256) con el protocolo Kerberos. Para que se puedan emitir TGT mediante AES, el nivel funcional del dominio debe ser Windows Server 2008 o superior y es necesario cambiar la contraseña del dominio.
    * Para obtener más información, consulta [Mejoras de Kerberos](/previous-versions/windows/it-pro/windows-vista/cc749438(v=ws.10)).

      > [!NOTE]
      >Pueden producirse errores de autenticación en un controlador de dominio después de que el nivel funcional del dominio se eleve a Windows Server 2008 o superior si el controlador de dominio ya ha replicado el cambio de DFL pero aún no ha actualizado la contraseña de krbtgt. En este caso, si se reinicia el servicio KDC en el controlador de dominio, se desencadenará una actualización en memoria de la nueva contraseña de krbtgt y se resolverán los errores de autenticación relacionados.

  * [La información del último inicio de sesión interactivo](https://go.microsoft.com/fwlink/?LinkId=180387) muestra lo siguiente:
     * El número total de intentos de inicio de sesión erróneos en un servidor Windows Server 2008 o una estación de trabajo con Windows Vista unidos a un dominio
     * El número total de intentos de inicio de sesión erróneos después de un inicio de sesión correcto en un servidor Windows Server 2008 o una estación de trabajo con Windows Vista
     * La duración de los intentos de inicio de sesión erróneos en un servidor Windows Server 2008 o una estación de trabajo con Windows Vista
     * La duración del último intento de inicio de sesión correcto en un servidor Windows Server 2008 o una estación de trabajo con Windows Vista
  * Las directivas de contraseña muy específicas permiten especificar directivas de bloqueo de cuentas y contraseñas para usuarios y grupos de seguridad global en un dominio. Para obtener más información, consulta la [Guía paso a paso para la configuración de directivas de bloqueo de cuentas y contraseñas muy específicas](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Escritorios virtuales personales
     * Para usar la funcionalidad agregada proporcionada por la pestaña Escritorio virtual personal del cuadro de diálogo Propiedades de cuenta de usuario en Usuarios y equipos de Active Directory, el esquema de AD DS debe ampliarse para Windows Server 2008 R2 (versión de objeto de esquema = 47). Para obtener más información, consulta la [Guía paso a paso para implementar escritorios virtuales personales mediante Conexión de RemoteApp y Escritorio](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema operativo de controlador de dominio admitido:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Características del nivel funcional de bosque de Windows Server 2003

* Todas las características de AD DS predeterminadas y las siguientes características están disponibles:
   * Confianza de bosque
   * Cambio de nombre de dominio
   * Replicación de valor vinculado
      - La replicación de valor vinculado te permite cambiar la pertenencia a grupos para almacenar y replicar los valores de los miembros individuales, en lugar de replicar toda la pertenencia como una sola unidad. El almacenamiento y la replicación de los valores de miembros individuales utiliza menos ancho de banda de red y menos ciclos de procesador durante la replicación, y evita que se pierdan actualizaciones cuando se agregan o quitan varios miembros simultáneamente en diferentes controladores de dominio.
   * La capacidad de implementar un controlador de dominio de solo lectura (RODC)
   * Mejora en la escalabilidad y los algoritmos del comprobador de coherencia de la información (KCC)
      - El generador de topología entre sitios (ISTG) usa algoritmos mejorados que se escalan para admitir bosques con un número mayor de sitios del que AD DS puede admitir en el nivel funcional del bosque de Windows 2000. El algoritmo mejorado de elección del ISTG es un mecanismo menos intrusivo para elegir el ISTG en el nivel funcional del bosque de Windows 2000.
   * La capacidad de crear instancias de la clase auxiliar dinámica denominada **dynamicObject** en una partición de directorio de dominio.
   * La capacidad de convertir una instancia de un objeto **inetOrgPerson** en una instancia de un objeto **User** , y para completar la conversión en la dirección opuesta.
   * La capacidad de crear instancias de nuevos tipos de grupos para admitir la autorización basada en roles.
      - Estos tipos se denominan grupos básicos de aplicación y grupos de consulta LDAP.
   * Desactivación y nueva definición de atributos y clases en el esquema. Se pueden reutilizar los siguientes atributos: ldapDisplayName, schemaIdGuid, OID y mapiID.
   * Los espacios de nombres de DFS basados en dominio que se ejecutan en modo Windows Server 2008, que incluyen compatibilidad con la enumeración basada en el acceso y mayor escalabilidad. Para obtener más información, consulta [Elegir un tipo de espacio de nombres](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Características del nivel funcional de dominio de Windows Server 2003

* Todas las características predeterminadas de AD DS, todas las características que están disponibles en el nivel funcional del dominio nativo de Windows Server 2000 y las características siguientes están disponibles:
   * La herramienta de administración de dominios, Netdom.exe, que permite cambiar el nombre de los controladores de dominio.
   * Actualizaciones de la marca de tiempo de inicio de sesión
      * El atributo **lastLogonTimestamp** se actualiza con la hora en que el usuario o equipo inició sesión por última vez. Este atributo se replica dentro del dominio.
   * La capacidad de establecer el atributo **userPassword** como la contraseña efectiva en el objeto **inetOrgPerson** y los objetos de usuario.
   * La capacidad de redirigir los contenedores Usuarios y equipos.
      * De manera predeterminada, se proporcionan dos contenedores conocidos para hospedar las cuentas del equipo y del usuario, a saber, cn=Computers,<domain root> y cn=Users,<domain root>. Esta característica permite definir una ubicación nueva conocida para estas cuentas.
   * La capacidad del Administrador de autorización para almacenar sus directivas de autorización en AD DS.
   * Delegación restringida
      * La delegación restringida hace posible que las aplicaciones aprovechen la delegación segura de credenciales de usuario por medio de la autenticación basada en Kerberos.
      * Solo puedes restringir la delegación a servicios de destino específicos.
   * Autenticación selectiva
      * La autenticación selectiva hace posible especificar los usuarios y grupos de un bosque de confianza a los que se les permite autenticarse en servidores de recursos en un bosque que confía.

## <a name="windows-2000"></a>Windows 2000

Sistema operativo de controlador de dominio admitido:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Características del nivel funcional de bosque de Windows Server 2000

* Todas las características de AD DS predeterminadas están disponibles.

### <a name="windows-2000-native-domain-functional-level-features"></a>Características del nivel funcional de dominio de Windows Server 2000

* Todas las características de AD DS predeterminadas y las siguientes características del directorio están disponibles, entre ellas:
   * Los grupos universales para grupos de distribución y de seguridad.
   * Anidación de grupos.
   * La conversión de grupos, que permite la conversión entre grupos de seguridad y grupos de distribución.
   * El historial de identificadores de seguridad (SID).

## <a name="next-steps"></a>Pasos a seguir

* [Elevar el nivel funcional del dominio](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753104(v=ws.11))
* [Elevar el nivel funcional del bosque](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730985(v=ws.11))