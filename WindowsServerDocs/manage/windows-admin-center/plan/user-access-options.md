---
title: Opciones de acceso de usuario con Windows Admin Center
description: Opciones de acceso de usuario y proveedores de identidad con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2019
ms.locfileid: "9152000"
---
# Opciones de acceso de usuario con Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Cuando se implementa en Windows Server, Windows Admin Center proporciona un punto centralizado de administración para el entorno de servidor. Al controlar el acceso al centro de administración de Windows, puedes mejorar la seguridad de su entorno de administración.

## Funciones de acceso de puerta de enlace

Windows Admin Center define dos funciones para acceder al servicio de puerta de enlace: los usuarios de la puerta de enlace y administradores de puerta de enlace.

> [!NOTE]
> Acceso a la puerta de enlace no implica el acceso a los servidores de destino visibles la puerta de enlace. Para administrar un servidor de destino, un usuario debe conectarse con las credenciales que tienen privilegios administrativos en el servidor de destino.

**Los usuarios de la puerta de enlace** puede conectarse al servicio de puerta de enlace de Windows Admin Center para administrar servidores a través de dicha puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación que se usan para autenticar a la puerta de enlace.

**Los administradores de puerta de enlace** puede configurar quién obtiene acceso, así como el comportamiento de los usuarios se autenticarán en la puerta de enlace.

>[!NOTE]
> Si no hay ningún grupos de acceso definidos en Windows Admin Center, los roles reflejará el acceso a la cuenta de Windows en el servidor de puerta de enlace. 

[Configurar el acceso de usuario y del Administrador de puerta de enlace de Windows Admin Center.](../configure/user-access-control.md)

## Opciones de proveedor de identidad

Los administradores de puerta de enlace pueden elegir uno de los siguientes:

 - [Grupos de equipos de Active Directory o local](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory como el proveedor de identidad para Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### Autenticación de tarjeta inteligente

Al usar Active Directory o grupos de equipo local como el proveedor de identidad, puede aplicar de autenticación de tarjeta inteligente, requerir que los usuarios que tienen acceso a Windows Admin Center para que sea un miembro de grupos de seguridad basada en la tarjeta inteligente adicionales. [Configurar la autenticación de tarjeta inteligente en Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### Acceso condicional y la autenticación multifactor

Al exigir autenticación de Azure AD para la puerta de enlace, puedes aprovechar las características de seguridad adicionales, como el acceso condicional y la autenticación multifactor proporcionado por Azure AD. [Más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Control de acceso basado en roles

De manera predeterminada, los usuarios necesitan privilegios de administrador local completa en las máquinas que desean administrar con Windows Admin Center.
Esto les permite conectarse de forma remota a la máquina y garantiza que tienen los permisos necesarios para ver y modificar la configuración del sistema.
Sin embargo, algunos usuarios no pueden necesitar acceso sin restricciones a la máquina para realizar su trabajo.
Puedes usar **el control de acceso basado en roles** en Windows Admin Center para proporcionar estos usuarios con acceso limitado a la máquina en lugar de hacer que los administradores locales completa.

Control de acceso basado en roles en Windows Admin Center funciona mediante la configuración de cada servidor administrado con un punto de conexión de PowerShell [Just Enough Administration](https://aka.ms/jeadocs) .
Este punto de conexión define las funciones, incluyendo cuáles son los aspectos del sistema de cada rol tiene permiso para administrar y los usuarios que se asignan a la función.
Cuando un usuario se conecta al punto de conexión restringido, se crea una cuenta de administrador local temporal para administrar el sistema en su nombre.
Esto garantiza que incluso herramientas que no tienen su propio modelo de delegación aún pueden administrarse con Windows Admin Center.
La cuenta temporal se elimina automáticamente cuando el usuario deja de administrar el equipo a través de Windows Admin Center.

Cuando un usuario se conecta a un equipo configurado con control de acceso basado en roles, Windows Admin Center primero comprueba si son un administrador local.
Si es así, recibirán la experiencia de Windows Admin Center completa sin restricciones.
De lo contrario, Windows Admin Center comprobará si el usuario pertenece a cualquiera de las funciones predefinidas.
Un usuario se dice que tiene *acceso limitado* si pertenecen a un rol de Windows Admin Center, pero no están un administrador completo.
Por último, si el usuario no es un administrador ni un miembro de un rol, se le denegará el acceso a administrar la máquina.

Control de acceso basado en roles está disponible para las soluciones de administrador del servidor y clúster de conmutación por error.

### Funciones disponibles

Windows Admin Center es compatible con los siguientes roles de usuario final:

Nombre de función | Uso previsto
----------|-------------
Administradores | Permite a los usuarios utilizar la mayoría de las características de Windows Admin Center sin concederles acceso a Escritorio remoto o PowerShell. Esta función es un buen recurso para "salto de servidor" escenarios donde quieres limitar los puntos de entrada de administración en una máquina.
Lectores | Permite a los usuarios ver información y la configuración en el servidor, pero no para realizar cambios.
Administradores de Hyper-V | Permite a los usuarios realizar cambios en las máquinas virtuales de Hyper-V y conmutadores, pero limita otras características de acceso de solo lectura.

Las siguientes extensiones integradas tienen funcionalidad reducida cuando un usuario conecta con acceso limitado:

- Archivos (no la carga de archivos o descarga)
- PowerShell (no disponible)
- Escritorio remoto (no disponible)
- Réplica de almacenamiento (no disponible)

En este momento, no puedes crear roles personalizados para la organización, pero puedes elegir qué usuarios tienen acceso a cada rol.

### Preparación para el control de acceso basado en roles

Para aprovechar las cuentas locales temporales, cada equipo de destino debe estar configurada para admitir el control de acceso basado en roles en Windows Admin Center.
El proceso de configuración implica la instalación de un punto de conexión de Just Enough Administration y scripts de PowerShell en la máquina con la configuración de estado deseado.

Si tienes solo unos pocos equipos, se puede aplicar fácilmente la configuración individualmente en cada equipo con la página de control de acceso basado en roles en Windows Admin Center.
Cuando se establece el control de acceso basado en roles en un equipo individual, se crean los grupos de seguridad local para controlar el acceso a cada rol.
Puede conceder acceso a los usuarios o grupos de seguridad de otros agregándolos como miembros de los grupos de seguridad de la función.

Para una implementación en toda la empresa en varias máquinas, puedes descargar el script de configuración de la puerta de enlace y distribuirlo a los equipos con un servidor de incorporación de cambios de configuración de estado deseado, automatización de Azure o sus herramientas de administración preferido.
