---
title: Implementar el sistema de archivos de red
description: Describe cómo implementar Network File System.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 761394f3190f02eedfea27a7d873c4c36535f23b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865116"
---
# <a name="deploy-network-file-system"></a>Implementar el sistema de archivos de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Network File System (NFS) proporciona una solución de uso compartido de archivos que permite transferir archivos entre equipos que ejecutan sistemas operativos Windows Server y UNIX mediante el protocolo NFS. En este tema se describen los pasos que debe seguir para implementar NFS.

## <a name="whats-new-in-network-file-system"></a>Novedades de Network File System

A continuación se explican los cambios de NFS en Windows Server 2012:

- **Compatibilidad con la versión 4,1 de NFS**. Esta versión del protocolo incluye las siguientes mejoras.
  - Desplazarse por los firewalls es más fácil, mejorando la accesibilidad.
  - Admite el protocolo\_GSS de RPCSEC, lo que proporciona una mayor seguridad y permite que los clientes y servidores negocien la seguridad.
  - Admite la semántica de archivos de UNIX y Windows.
  - Aprovecha las implementaciones de servidor de archivos en clúster.
  - Admite procedimientos compuestos compuestas para WAN.

- **Módulo NFS para Windows PowerShell**. La disponibilidad de los cmdlets integrados de NFS facilita la automatización de diversas operaciones. Los nombres de los cmdlets son coherentes con otros cmdlets de Windows PowerShell (mediante verbos como "Get" y "SET"), lo que facilita que los usuarios familiarizados con Windows PowerShell aprendan a usar nuevos cmdlets.
- **Mejoras**en la administración de NFS. Una nueva consola de administración centralizada basada en interfaz de usuario simplifica la configuración y la administración de recursos compartidos de SMB y NFS, cuotas, filtros de archivos y clasificaciones, además de administrar servidores de archivos en clúster.
- **Mejoras en la asignación de identidades**. Nueva compatibilidad con la interfaz de usuario y cmdlets de Windows PowerShell basados en tareas para configurar la asignación de identidades, lo que permite a los administradores configurar rápidamente un origen de asignación de identidades y, a continuación, crear identidades asignadas individuales para los usuarios. Las mejoras facilitan a los administradores la configuración de un recurso compartido para el acceso de varios protocolos a través de NFS y SMB.
- **Reestructuración del modelo de recursos del clúster**. Esta mejora aporta coherencia entre el modelo de recursos de clúster para los servidores de protocolo NFS y SMB de Windows y simplifica la administración. En el caso de los servidores NFS que tienen muchos recursos compartidos, la red de recursos y el número de llamadas WMI necesarias conmutarán por error un volumen que contenga un gran número de recursos compartidos de NFS.
- **Integración con el administrador de claves de reanudación**. El administrador de claves de reanudación es un componente que realiza un seguimiento del servidor de archivos y el estado del sistema de archivos y permite que los servidores de protocolo SMB y NFS de Windows conmuten por error sin interrumpir a los clientes o aplicaciones de servidor que almacenan sus datos en el servidor de archivos. Esta mejora es un componente clave de la capacidad de disponibilidad continua del servidor de archivos que ejecuta Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Escenarios para el uso del sistema de archivos de red

NFS es compatible con un entorno mixto de sistemas operativos basados en Windows y UNIX. Los siguientes escenarios de implementación son ejemplos de cómo puede implementar un servidor de archivos de Windows Server 2012 continuamente disponible mediante NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Aprovisionar recursos compartidos de archivos en entornos heterogéneos

Este escenario se aplica a organizaciones con entornos heterogéneos que constan de Windows y otros sistemas operativos, como equipos cliente UNIX o Linux. Con este escenario, puede proporcionar acceso multiprotocolo al mismo recurso compartido de archivos a través de los protocolos SMB y NFS. Normalmente, cuando se implementa un servidor de archivos de Windows en este escenario, se desea facilitar la colaboración entre los usuarios de equipos basados en Windows y UNIX. Cuando se configura un recurso compartido de archivos, se comparte con los protocolos SMB y NFS, con los usuarios de Windows que tienen acceso a sus archivos a través del protocolo SMB y los usuarios de equipos basados en UNIX suelen tener acceso a sus archivos a través del protocolo NFS.

Para este escenario, debe tener una configuración de origen de asignación de identidad válida. Windows Server 2012 admite los siguientes almacenes de asignación de identidades:

- Archivo de asignación
- Servicios de dominio de Active Directory (AD DS)
- Almacenes LDAP compatibles con RFC 2307, como Active Directory Lightweight Directory Services (AD LDS)
- Servidor Asignación de nombres de usuario (UNM)

### <a name="provision-file-shares-in-unix-based-environments"></a>Aprovisionar recursos compartidos de archivos en entornos basados en UNIX

En este escenario, los servidores de archivos de Windows se implementan en un entorno principalmente basado en UNIX para proporcionar acceso a recursos compartidos de archivos NFS para equipos cliente basados en UNIX. Una opción de acceso de usuario de UNIX (UUUA) no asignada se implementó inicialmente para recursos compartidos de NFS en Windows Server 2008 R2, de modo que los servidores de Windows pueden usarse para almacenar datos NFS sin crear la asignación de cuentas de UNIX a Windows. UUUA permite a los administradores aprovisionar e implementar rápidamente NFS sin tener que configurar la asignación de cuentas. Cuando se habilita para NFS, UUUA crea identificadores de seguridad (SID) personalizados para representar a los usuarios sin asignar. Las cuentas de usuario asignadas usan los identificadores de seguridad de Windows (SID) estándar y los usuarios sin asignar usan SID de NFS personalizados.

## <a name="system-requirements"></a>Requisitos del sistema

Servidor para NFS se puede instalar en cualquier versión de Windows Server 2012. Puede usar NFS con equipos basados en UNIX que ejecuten un servidor NFS o un cliente NFS si estas implementaciones de cliente y servidor NFS cumplen con una de las siguientes especificaciones de protocolo:

1. Especificación del protocolo NFS versión 4,1 (tal y como se define en RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Especificación del protocolo NFS versión 3 (tal y como se define en RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Especificación del protocolo NFS versión 2 (tal y como se define en RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Implementar la infraestructura de NFS

Debe implementar los siguientes equipos y conectarlos en una red de área local (LAN):

- Uno o más equipos que ejecutan Windows Server 2012 en los que va a instalar los dos servicios principales de los componentes de NFS: Servidor para NFS y cliente para NFS. Puede instalar estos componentes en el mismo equipo o en equipos diferentes.
- Uno o varios equipos basados en UNIX que ejecutan el software de cliente NFS y de servidor NFS. El equipo basado en UNIX que ejecuta el servidor NFS hospeda un recurso compartido de archivos NFS, al que se tiene acceso desde un equipo que ejecuta Windows Server 2012 como cliente con cliente para NFS. Puede instalar el software de cliente y servidor NFS en el mismo equipo basado en UNIX o en diferentes equipos basados en UNIX, según desee.
- Un controlador de dominio que se ejecuta en el nivel funcional de Windows Server 2008 R2. El controlador de dominio proporciona la información de autenticación del usuario y la asignación para el entorno de Windows.
- Cuando un controlador de dominio no está implementado, puede utilizar un servidor NIS (NIS) para proporcionar información de autenticación del usuario para el entorno de UNIX. O bien, si lo prefiere, puede usar archivos de grupos y contraseñas que estén almacenados en el equipo que ejecuta el servicio de Asignación de nombres de usuario.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Instale Network File System en el servidor con Administrador del servidor

1. En el Asistente para agregar roles y características, en Roles de servidor, selecciona **Servicios de archivos y almacenamiento** (si todavía no se ha instalado).
2. En **servicios de archivos e iSCSI**, seleccione **servidor de archivos** y **servidor para NFS**. Seleccione **Agregar características** para incluir las características de NFS seleccionadas.
3. Seleccione **instalar** para instalar los componentes de NFS en el servidor.

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>Instalar Network File System en el servidor con Windows PowerShell

1. Inicie Windows PowerShell. Haz clic con el botón derecho en el icono de PowerShell de la barra de tareas y selecciona **Ejecutar como administrador**.
2. Ejecute los siguientes comandos de Windows PowerShell:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>Configuración de la autenticación NFS

Al usar los protocolos NFS versión 4,1 y versión 3,0 de NFS, tiene las siguientes opciones de autenticación y seguridad.

- RPCSEC\_GSS
  - **Krb5**. Usa el protocolo Kerberos versión 5 para autenticar a los usuarios antes de conceder acceso al recurso compartido de archivos.
  - **Krb5i**. Usa el protocolo Kerberos versión 5 para autenticarse con comprobación de integridad (sumas de comprobación), que comprueba que los datos no se han modificado.
  - **Krb5p** Usa el protocolo Kerberos versión 5, que autentica el tráfico NFS con cifrado de privacidad.
- AUTENTICACIÓN\_SYS

También puede elegir no usar la autorización del servidor (autenticación\_sys), lo que le ofrece la opción de habilitar el acceso de usuario sin asignar. Al usar el acceso de usuario sin asignar, puede especificar para permitir el acceso de usuario sin asignar por UID/GID, que es el valor predeterminado, o permitir el acceso anónimo.

Las instrucciones para configurar la autenticación de NFS en se describen en la sección siguiente.

## <a name="create-an-nfs-file-share"></a>Creación de un recurso compartido de archivos NFS

Puede crear un recurso compartido de archivos NFS mediante Administrador del servidor o cmdlets de NFS de Windows PowerShell.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Cree un recurso compartido de archivos NFS con Administrador del servidor

1. Inicie sesión en el servidor como miembro del grupo Administradores local.
2. El Administrador del servidor se iniciará de manera automática. Si no se inicia automáticamente, seleccione **iniciar**, escriba **ServerManager. exe**y, a continuación, seleccione **Administrador del servidor**.
3. A la izquierda, seleccione **servicios de archivos y almacenamiento**y, a continuación, seleccione **recursos compartidos**.
4. Seleccione **esta acción para crear un recurso compartido de archivos, inicie el Asistente para nuevo recurso compartido**.
5. En la página **Seleccionar perfil** , seleccione **recurso compartido de NFS – rápido** o **recurso compartido de NFS-avanzado**y, a continuación, seleccione **siguiente**.
6. En la página **Ubicación del recurso compartido** , seleccione un servidor y un volumen y seleccione **siguiente**.
7. En la página **nombre del recurso compartido** , especifique un nombre para el nuevo recurso compartido y seleccione **siguiente**.
8. En la página **autenticación** , especifique el método de autenticación que desea usar para este recurso compartido.
9. En la página **permisos de recurso compartido** , seleccione **Agregar**y, a continuación, especifique el host, el grupo de cliente o el netgroup al que desea conceder permiso para el recurso compartido.
10. En **permisos**, configure el tipo de control de acceso que desea que tengan los usuarios y seleccione **Aceptar**.
11. En la página **confirmación** , revise la configuración y seleccione **crear** para crear el recurso compartido de archivos NFS.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

El siguiente cmdlet de Windows PowerShell también puede crear un recurso compartido de archivos `nfs1` NFS (donde es el nombre del `C:\\shares\\nfsfolder` recurso compartido y es la ruta de acceso del archivo):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problema conocido
La versión 4,1 de NFS permite crear o copiar los nombres de archivo con caracteres no válidos. Si intenta abrir los archivos con el editor vi, se muestra como dañados. No se puede guardar el archivo desde VI, cambiar su nombre, moverlo o cambiar los permisos. Evite el uso de caracteres no válidos.
