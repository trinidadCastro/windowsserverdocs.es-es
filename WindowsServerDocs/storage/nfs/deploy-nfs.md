---
title: Implementar el sistema de archivos de red
description: Se describe cómo implementar el sistema de archivos de red.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976691"
---
# Implementar el sistema de archivos de red

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Network File System (NFS) proporciona una solución que te permite transferir archivos entre equipos que ejecutan Windows Server y sistemas operativos UNIX mediante el protocolo NFS de uso compartido de archivos. En este tema se describen los pasos que debe seguir para implementar NFS.

## Novedades de sistema de archivos de red

Aquí es lo que se ha cambiado para NFS en Windows Server 2012:

- **Soporte para NFS versión 4.1**. Esta versión de protocolo incluye las siguientes mejoras.
  - Navegar por los servidores de seguridad es más fácil, mejorar la accesibilidad.
  - Admite el protocolo RPCSEC\_GSS, proporcionando una mayor seguridad y lo que permite a los clientes y servidores negociar la seguridad.
  - Admite la semántica de archivo de UNIX y Windows.
  - Aprovecha las ventajas de las implementaciones de servidor de archivos agrupados en clúster.
  - Es compatible con los procedimientos compuestos WAN cuentan con asistencia.

- **Módulo NFS para Windows PowerShell**. La disponibilidad de cmdlets integrados de NFS facilita la automatizar diversas operaciones. Los nombres de cmdlet son coherentes con otros cmdlets de Windows PowerShell (con verbos como "Obtener" y "Set"), lo que sea más fácil para los usuarios familiarizados con Windows PowerShell para aprender a usar los cmdlets de nuevos.
- **Mejoras de administración de NFS**. Una consola de administración centralizada basada en la interfaz de usuario nueva simplifica la configuración y administración de SMB y recursos compartidos NFS, cuotas, filtros de archivos y clasificación, además de administrar los servidores de archivos agrupados en clúster.
- **Mejoras de asignación de identidad**. Nueva compatibilidad de la interfaz de usuario y basada en tareas de los cmdlets de Windows PowerShell para configurar la asignación de identidad, lo que permite a los administradores configurar rápidamente un origen de asignación de identidad y, a continuación, crear identidades asignadas individuales para los usuarios. Mejoras que resulte más fácil a los administradores configurar un recurso compartido para acceso de múltiples protocolos a través de NFS y SMB.
- **Modelo de recurso de clúster reestructurar**. Esta mejora aporta coherencia entre el modelo de recurso de clúster para NFS de Windows y servidores de protocolo SMB y simplifica la administración. Para los servidores NFS que tienen muchos recursos compartidos, la red de recursos y el número de llamadas WMI necesario producirá un error a través de un volumen que contiene que una gran cantidad de recursos compartidos de NFS se reduce.
- **Integración con el Administrador de claves de reanudación**. El Administrador de clave de reanudación es un componente que realiza un seguimiento de servidor de archivos y el estado del sistema de archivos y permite a los servidores de protocolo SMB de Windows y NFS conmutación por error sin interrumpir clientes o aplicaciones de servidor que almacenan sus datos en el servidor de archivos. Esta mejora es un componente clave de la capacidad de disponibilidad continua del servidor de archivos que ejecutan Windows Server 2012.

## Escenarios de uso de sistema de archivos de red

NFS admite un entorno mixto de sistemas operativos basados en Windows y basados en UNIX. Los siguientes escenarios de implementación son ejemplos de cómo se puede implementar un servidor de archivos de Windows Server 2012 disponible continuamente con NFS.

### Recursos compartidos de archivos de aprovisionar en entornos heterogéneos

Este escenario se aplica a las organizaciones que tienen entornos heterogéneos que constan de equipos Windows y otros sistemas operativos, como cliente basado en Linux o UNIX equipos. Con este escenario, puedes proporcionar acceso de múltiples protocolos para el mismo recurso compartido de archivos a través de protocolos la SMB y NFS. Por lo general, al implementar un servidor de archivos de Windows en este escenario, quieres facilitar la colaboración entre equipos basados en UNIX y los usuarios de Windows. Cuando se configura un recurso compartido de archivos, se comparte con los SMB NFS protocolos y, con los usuarios de Windows para obtener acceso a sus archivos mediante el protocolo SMB, y los usuarios de equipos basados en UNIX por lo general, acceder a sus archivos a través del protocolo NFS.

Para este escenario, debes tener una configuración de origen de asignación de identidad válida. Windows Server 2012 admite los siguientes almacenes de asignación de identidad:

- Archivo de asignación
- Servicios de dominio de Active Directory (AD DS)
- RFC 2307 conforme LDAP se almacena como Active Directory Lightweight Directory Services (AD LDS)
- Servidor de asignación de nombre (UNM) de usuario

### Recursos compartidos de archivos de aprovisionar en entornos basados en UNIX

En este escenario, los servidores de archivos de Windows se implementan en un entorno de UNIX-based fundamentalmente para proporcionar acceso a recursos compartidos de archivos NFS para los equipos cliente basados en UNIX. Una opción de acceso de usuario de UNIX sin asignar (UUUA) se ha implementado inicialmente para recursos compartidos NFS en Windows Server 2008 R2 para que la asignación de cuentas de Windows, los servidores pueden usarse para almacenar datos de NFS sin tener que crear UNIX a Windows. UUUA permite a los administradores aprovisionar rápidamente NFS sin tener que configurar la asignación de cuenta. Cuando se habilita para NFS, UUUA crea identificadores de seguridad personalizado (SID) para representar los usuarios sin asignar. Las cuentas de usuario asignado usan identificadores de seguridad (SID) de Windows estándares y sin asignar a los usuarios usan SID de NFS personalizados.

## Requisitos del sistema

Servidor para NFS se puede instalar en cualquier versión de Windows Server 2012. Puedes usar NFS con equipos basados en UNIX que ejecutan un cliente para NFS o servidor NFS si estas implementaciones de servidor y cliente NFS cumplan con una de las especificaciones de protocolo siguientes:

1. NFS versión 4.1 la especificación protocolo (como se define en RFC [5661](https://tools.ietf.org/html/rfc5661))
2. NFS versión 3 la especificación protocolo (como se define en RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Protocolo de especificación de NFS versión 2 (como se define en RFC [1094](https://tools.ietf.org/html/rfc1094))

## Implementar la infraestructura NFS

Debes implementar los siguientes equipos y conectarse a ellos en una red de área local (LAN):

- Uno o varios de los equipos que ejecutan Windows Server 2012 en el que se instalará los dos servicios principales para los componentes NFS: servidor para NFS y cliente para NFS. Puedes instalar estos componentes en el mismo equipo o en equipos diferentes.
- Uno o más basados en UNIX equipos que ejecutan servidor NFS y software de cliente NFS. El equipo basado en UNIX que se está ejecutando servidor NFS hospeda un recurso compartido de archivos NFS o de exportación, que se accede a un equipo que ejecute Windows Server 2012 como un cliente con el cliente para NFS. Para instalar el software de cliente y servidor NFS, en el mismo equipo basado en UNIX o en equipos basados en UNIX distintos, según tus preferencias.
- Un controlador de dominio que se ejecuta en el nivel funcional de Windows Server 2008 R2. El controlador de dominio proporciona información de autenticación de usuario y la asignación para el entorno de Windows.
- Cuando no se implementa un controlador de dominio, puedes usar un servidor de servicio de información de red (NIS) para proporcionar información de autenticación de usuario para el entorno de UNIX. O bien, si lo prefieres, puedes usar contraseña y grupo de archivos que se almacenan en el equipo que se está ejecutando el servicio de asignaciones de nombre de usuario.

### Instalar el sistema de archivos de red en el servidor con el administrador del servidor

1. En el agregar Roles and Features Wizard, en funciones de servidor, selecciona **File and Storage Services** si ya no se ha instalado.
2. En **iSCSI y archivo servicios**, selecciona el **Servidor de archivos** y **servidor para NFS**. Selecciona **Agregar características** para incluir características NFS seleccionadas.
3. Selecciona **instalar** para instalar los componentes NFS en el servidor.

### Instalar el sistema de archivos de red en el servidor con Windows PowerShell

1. Inicie Windows PowerShell. Haz clic en el icono de PowerShell en la barra de tareas y seleccione **Ejecutar como administrador**.
2. Ejecute los siguientes comandos de Windows PowerShell:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## Configurar la autenticación de NFS

Al usar la versión NFS 4.1 y protocolos de versión 3.0 NFS, tienes las siguientes opciones de autenticación y seguridad.

- RPCSEC\_GSS
  - **Krb5**. Usa el protocolo de la versión 5 de Kerberos para autenticar a los usuarios antes de conceder acceso al recurso compartido de archivo.
  - **Krb5i**. Usa el protocolo de la versión 5 de Kerberos para autenticar con la integridad de comprobación (sumas de comprobación), que comprueba que no se han manipulado los datos.
  - **Krb5p** Usa el protocolo de la versión 5 de Kerberos, que autentica el tráfico NFS con el cifrado de privacidad.
- AUTH\_SYS

También puedes no se debe utilizar la autorización del servidor (AUTH\_SYS), lo que proporciona la opción de habilitar el acceso de usuario sin asignar. Al usar el acceso de usuario sin asignar, puedes especificar para permitir el acceso a un usuario sin asignar UID / GID, que es el valor predeterminado, o permitir el acceso anónimo.

Instrucciones para configurar la autenticación de NFS en que se describen en la siguiente sección.

## Crear un recurso compartido de NFS

Puedes crear un recurso compartido de archivos NFS mediante los cmdlets de administrador del servidor o Windows PowerShell NFS.

### Crear un recurso compartido de NFS con el administrador del servidor

1. Inicia sesión en el servidor como miembro del grupo de administradores locales.
2. Administrador de servidores se iniciará automáticamente. Si no se inicia automáticamente, selecciona el **Inicio**, escribe **servermanager.exe**y, a continuación, selecciona el **Administrador del servidor**.
3. A la izquierda, selecciona **File and Storage Services**y, a continuación, selecciona **los recursos compartidos**.
4. Selecciona **para crear un recurso compartido de archivos, inicie el Asistente para nuevo recurso compartido**.
5. En la página **Seleccionar perfil** , selecciona **Rápido recurso compartido de NFS** o **recurso compartido de NFS - avanzada**y luego selecciona **siguiente**.
6. En la página **Ubicación de recurso compartido** , selecciona un servidor y un volumen y seleccione **siguiente**.
7. En la página de **Nombre del recurso compartido** , especifica un nombre para el recurso compartido de nuevo y selecciona **siguiente**.
8. En la página de **autenticación** , especifica el método de autenticación que quieras usar para este recurso compartido.
9. En la página de **Permisos de recurso compartido** , selecciona **Agregar**y, a continuación, especificar el host, grupo de clientes o netgroup que quieres conceder permiso para el recurso compartido.
10. En los **permisos**, configurar el tipo de control de acceso que quieres que los usuarios tengan y selecciona **Aceptar**.
11. En la página de **confirmación** , revisa la configuración y seleccione **crear** para crear el recurso compartido de archivos NFS.

### Comandos equivalentes de Windows PowerShell

El siguiente cmdlet de Windows PowerShell también puede crear un recurso compartido de NFS (donde `nfs1` es el nombre del recurso compartido y `C:\\shares\\nfsfolder` es la ruta de acceso de archivo):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### Problema conocido
NFS versión 4.1 permite que los nombres de archivo se crean o se copian con caracteres no permitidos. Si se intenta abrir los archivos con el editor vi, se muestra como que se está dañado. No guardar el archivo de vi, cambia el nombre, moverlo o cambiar los permisos. Evita usar caracteres no válido.
