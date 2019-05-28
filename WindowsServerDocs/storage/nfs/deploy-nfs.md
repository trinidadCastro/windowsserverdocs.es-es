---
title: Implementar el sistema de archivos de red
description: Describe cómo implementar el sistema de archivos de red.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: ab80b6d73a40256d5935635c9afc55b7c53727d3
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63737822"
---
# <a name="deploy-network-file-system"></a>Implementar el sistema de archivos de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Network File System (NFS) proporciona una solución que le permite transferir archivos entre equipos que ejecutan Windows Server y sistemas operativos UNIX mediante el protocolo NFS para compartir de archivos. En este tema se describen los pasos que debe seguir para implementar NFS.

## <a name="whats-new-in-network-file-system"></a>Novedades de Network File System

Aquí es lo que ha cambiado para NFS en Windows Server 2012:

- **Compatibilidad con NFS versión 4.1**. Esta versión de protocolo incluye las siguientes mejoras.
  - Navegar por los servidores de seguridad es más fácil, mejorar la accesibilidad.
  - Admite la RPCSEC\_protocolo GSS, que proporciona una mayor seguridad y permitir que los clientes y servidores negociar la seguridad.
  - Admite la semántica de archivos UNIX y Windows.
  - Aprovecha las ventajas de las implementaciones de servidor de archivos en clúster.
  - Admite procedimientos compuestos adaptada a WAN.

- **Módulo NFS para Windows PowerShell**. La disponibilidad de los cmdlets NFS integrados resulta más fácil automatizar diversas operaciones. Los nombres de cmdlet son coherentes con otros Windows cmdlets de PowerShell (con verbos como "Get" y "Set"), que permiten a los usuarios familiarizados con Windows PowerShell para aprender a usar los nuevos cmdlets.
- **Mejoras en la administración de NFS**. Una nueva consola de administración centralizada basada en la interfaz de usuario simplifica la configuración y administración de SMB y recursos compartidos NFS, cuotas, filtros de archivos y la clasificación, además de administrar servidores de archivos en clúster.
- **Mejoras de asignación de identidad**. Nueva compatibilidad de interfaz de usuario y los cmdlets de Windows PowerShell basada en tareas para configurar la asignación de identidad, que permite a los administradores configurar rápidamente un origen de asignación de identidad y, a continuación, crear identidades asignadas individuales para los usuarios. Mejoras facilitan a los administradores configurar un recurso compartido para acceso de múltiples protocolos a través de SMB y NFS.
- **Reestructuración de modelo de recursos de clúster**. Esta mejora aporta coherencia entre el modelo de recursos de clúster para el NFS de Windows y los servidores del protocolo SMB y simplifica la administración. Para los servidores NFS que tienen muchos de los recursos compartidos, la red de recursos y el número de llamadas WMI necesarias conmutación por error un volumen que contiene que un gran número de recursos compartidos de NFS se reduce.
- **Integración con el Administrador de claves de reanudación**. El Administrador de la clave de reanudación es un componente que realiza el seguimiento de servidor de archivos y el estado del sistema de archivos y permite que los servidores del protocolo Windows SMB y NFS conmutar por error sin interrumpir a los clientes o aplicaciones de servidor que almacenan sus datos en el servidor de archivos. Esta mejora es un componente clave de la funcionalidad de la disponibilidad continua del servidor de archivos que ejecutan Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Escenarios de uso de Network File System

NFS es compatible con un entorno mixto de los sistemas operativos Windows y UNIX. Los siguientes escenarios de implementación son ejemplos de cómo puede implementar un servidor de archivos de Windows Server 2012 disponible continuamente mediante NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Recursos compartidos de archivos de aprovisionamiento en entornos heterogéneos

Este escenario se aplica a las organizaciones con entornos heterogéneos que constan de Windows y otros sistemas operativos, como cliente basado en Linux o UNIX equipos. Con este escenario, puede proporcionar acceso de protocolo múltiple para el mismo recurso compartido de archivos a través de protocolos el SMB y NFS. Normalmente, al implementar un servidor de archivos de Windows en este escenario, desea facilitar la colaboración entre los usuarios de Windows y equipos basados en UNIX. Cuando se configura un recurso compartido de archivos, se comparte con protocolos el SMB y NFS, con usuarios de Windows tienen acceso a sus archivos a través del protocolo SMB, y los usuarios de equipos basados en UNIX suelen tener acceso a sus archivos a través del protocolo NFS.

En este escenario, debe tener una configuración de origen de asignación de identidad válida. Windows Server 2012 admite los siguientes almacenes de asignación de identidad:

- Archivo de asignación
- Servicios de dominio de Active Directory (AD DS)
- RFC 2307 compatible con LDAP que se almacena como Active Directory Lightweight Directory Services (AD LDS)
- Servidor de asignación de nombre (UNM) del usuario

### <a name="provision-file-shares-in-unix-based-environments"></a>Recursos compartidos de archivos de aprovisionamiento en entornos basados en UNIX

En este escenario, los servidores de archivos de Windows se implementan en un entorno fundamentalmente basados en UNIX para proporcionar acceso a recursos compartidos de archivos NFS para los equipos cliente basados en UNIX. Inicialmente se ha implementado una opción de usuario acceso a UNIX (UUUA) recursos compartidos de NFS en Windows Server 2008 R2 para que la asignación de cuentas de Windows, los servidores se pueden usar para almacenar datos NFS sin crear UNIX a Windows. UUUA permite a los administradores aprovisionar e implementar con rapidez NFS sin tener que configurar la asignación de cuenta. Cuando se habilita para NFS, UUUA crea personalizado seguridad (SID) para representar los usuarios sin asignar. Cuentas de usuario asignadas utilizan identificadores estándares de seguridad de Windows (SID) y los usuarios sin asignar los SID de NFS personalizados.

## <a name="system-requirements"></a>Requisitos del sistema

Servidor para NFS se puede instalar en cualquier versión de Windows Server 2012. Puede usar NFS con equipos basados en UNIX que ejecutan un servidor NFS o cliente para NFS si se cumplen estas implementaciones de cliente y servidor NFS con una de las siguientes especificaciones de protocolo:

1. Especificación del protocolo NFS versión 4.1 (tal como se define en RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Especificación del protocolo NFS versión 3 (como se define en RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Especificación del protocolo NFS versión 2 (como se define en RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Implementar la infraestructura NFS

Deberá implementar los siguientes equipos y conectarse a ellos en una red de área local (LAN):

- Uno o más equipos que ejecutan Windows Server 2012 en el que se instalará el dos principales componentes de servicios para NFS: Servidor para NFS y cliente para NFS. Puede instalar estos componentes en el mismo equipo o en equipos diferentes.
- Uno o más equipos UNIX que ejecutan servidor NFS y software de cliente NFS. El equipo basado en UNIX que se está ejecutando el servidor NFS hospeda un recurso compartido de archivos NFS o exportación, que se tiene acceso a un equipo que ejecuta Windows Server 2012 como un cliente con cliente para NFS. Puede instalar software cliente y servidor NFS en el mismo equipo basado en UNIX o en diferentes equipos basados en UNIX, según sea necesario.
- Un controlador de dominio en el nivel funcional de Windows Server 2008 R2. El controlador de dominio proporciona información de autenticación de usuario y la asignación para el entorno de Windows.
- Cuando no se implementa un controlador de dominio, puede usar un servidor de servicio de información de red (NIS) para proporcionar información de autenticación de usuario para el entorno de UNIX. O bien, si lo prefiere, puede usar la contraseña y grupo de archivos que se almacenan en el equipo que ejecuta el servicio de asignación de nombres de usuario.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Instalar el sistema de archivos de red en el servidor con el administrador del servidor

1. En el Asistente para agregar roles y características, en Roles de servidor, selecciona **Servicios de archivos y almacenamiento** (si todavía no se ha instalado).
2. En **iSCSI y archivo servicios**, seleccione **servidor de archivos** y **servidor para NFS**. Seleccione **agregar características** para incluir las características seleccionadas de NFS.
3. Seleccione **instalar** para instalar los componentes NFS en el servidor.

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>Instalar el sistema de archivos de red en el servidor con Windows PowerShell

1. Inicie Windows PowerShell. Haz clic con el botón derecho en el icono de PowerShell de la barra de tareas y selecciona **Ejecutar como administrador**.
2. Ejecute los siguientes comandos de Windows PowerShell:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>Configurar la autenticación de NFS

Cuando se usa la versión 4.1 de NFS y protocolos de la versión 3.0 de NFS, tiene las siguientes opciones de autenticación y seguridad.

- RPCSEC\_GSS
  - **Krb5**. Usa el protocolo Kerberos versión 5 para autenticar usuarios antes de conceder acceso al recurso compartido de archivos.
  - **Krb5i**. Utiliza el protocolo Kerberos versión 5 para autenticarse con integridad (sumas de comprobación), la comprobación que comprueba que los datos no ha sido alterados.
  - **Krb5p** protocolo de usa Kerberos versión 5, que autentica el tráfico NFS con cifrado para proteger la privacidad.
- AUTENTICACIÓN\_SYS

También puede elegir no usar la autorización del servidor (AUTH\_SYS), que ofrece la opción para habilitar el acceso de usuarios sin asignar. Al usar el acceso de usuarios sin asignar, puede especificar para permitir el acceso a usuarios sin asignar UID y GID, que es el valor predeterminado o permitir el acceso anónimo.

Instrucciones para configurar la autenticación de NFS en que se describen en la sección siguiente.

## <a name="create-an-nfs-file-share"></a>Crear un recurso compartido NFS

Puede crear un recurso compartido NFS con cmdlets del administrador del servidor o Windows PowerShell NFS.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Crear un recurso compartido NFS con el administrador del servidor

1. Inicie sesión en el servidor como miembro del grupo Administradores local.
2. El Administrador del servidor se iniciará de manera automática. Si no se inicia automáticamente, seleccione **iniciar**, tipo **servermanager.exe**y, a continuación, seleccione **administrador del servidor**.
3. En el lado izquierdo, seleccione **File and Storage Services**y, a continuación, seleccione **recursos compartidos**.
4. Seleccione **para crear un recurso compartido de archivos, inicie el Asistente para nuevo recurso compartido**.
5. En el **Seleccionar perfil** página, seleccione **rápido de recurso compartido de NFS** o **recurso compartido NFS - avanzado**, a continuación, seleccione **siguiente**.
6. En el **ubicación del recurso compartido** página, seleccione un servidor y un volumen y seleccione **siguiente**.
7. En el **nombre del recurso compartido** página, especifique un nombre para el nuevo recurso compartido y seleccione **siguiente**.
8. En el **autenticación** , especifique el método de autenticación que desea usar para este recurso compartido.
9. En el **compartir permisos** página, seleccione **agregar**y, a continuación, especifique el host, el grupo de clientes o netgroup que desea conceder permiso para el recurso compartido.
10. En **permisos**, configurar el tipo de control de acceso que desea que los usuarios que tienen y seleccione **Aceptar**.
11. En el **confirmación** página, revise la configuración y seleccione **crear** para crear el recurso compartido de archivos NFS.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

El siguiente cmdlet de Windows PowerShell también puede crear un recurso compartido NFS (donde `nfs1` es el nombre del recurso compartido y `C:\\shares\\nfsfolder` es la ruta de acceso de archivo):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problema conocido
Versión 4.1 de NFS permite a los nombres de archivo se crean o se copian con caracteres no válidos. Si intenta abrir los archivos con el editor vi, se muestra como estén dañados. No se puede guardar el archivo de vi, cambie el nombre, moverlo o cambiar permisos. Evite usar caracteres no válido.
