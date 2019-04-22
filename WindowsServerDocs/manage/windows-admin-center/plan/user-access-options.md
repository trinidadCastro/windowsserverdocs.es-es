---
title: Opciones de acceso de usuario con Windows Admin Center
description: Opciones de acceso de usuario y los proveedores de identidades con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825896"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opciones de acceso de usuario con Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Cuando se implementa en Windows Server, Windows Admin Center proporciona un punto centralizado de administración para su entorno de servidor. Controlar el acceso a Windows Admin Center, puede mejorar la seguridad de su infraestructura de administración.

## <a name="gateway-access-roles"></a>Roles de acceso de la puerta de enlace

Windows Admin Center define dos roles para el acceso al servicio de puerta de enlace: los usuarios de la puerta de enlace y los administradores de la puerta de enlace.

> [!NOTE]
> Acceso a la puerta de enlace no implica el acceso a los servidores de destino visibles por la puerta de enlace. Para administrar un servidor de destino, debe conectar un usuario con credenciales que tengan privilegios administrativos en el servidor de destino.

**Los usuarios de la puerta de enlace** pueden conectarse al servicio de puerta de enlace de Windows Admin Center para administrar los servidores a través de esa puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación usada para autenticarse en la puerta de enlace.

**Los administradores de la puerta de enlace** puede configurar quién tiene acceso también cómo se autenticarán los usuarios a la puerta de enlace.

>[!NOTE]
> Si no hay ningún grupo de acceso definido en Windows Admin Center, los roles reflejará el acceso a la cuenta de Windows en el servidor de puerta de enlace. 

[Configurar el acceso de usuario y Administrador de puerta de enlace en Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opciones de proveedor de identidad

Los administradores de la puerta de enlace pueden elegir cualquiera de las siguientes acciones:

 - [Grupos de equipos de Active Directory o local](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory como proveedor de identidades de Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticación de tarjeta inteligente

Al usar grupos de equipo local o de Active Directory como proveedor de identidades, puede exigir la autenticación de tarjeta inteligente al requerir que los usuarios que tienen acceso a Windows Admin Center para que sea miembro de grupos de seguridad adicional basada en tarjeta inteligente. [Configurar la autenticación de tarjeta inteligente en Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Acceso condicional y Multi-factor authentication

Al requerir autenticación de Azure AD para la puerta de enlace, puede aprovechar las características de seguridad adicionales, como el acceso condicional y Multi-factor authentication proporcionada Azure AD. [Más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Control de acceso basado en roles

De forma predeterminada, los usuarios necesitan privilegios completos de administrador local en los equipos que deseen administrar mediante Windows Admin Center.
Esto les permita conectarse remotamente a la máquina y se asegura de que tienen permisos suficientes para ver y modificar la configuración del sistema.
Sin embargo, algunos usuarios no pueden necesitar acceso sin restricciones a la máquina para realizar sus trabajos.
Puede usar **control de acceso basado en roles** en Windows Admin Center para proporcionar estos usuarios con acceso limitado a la máquina en lugar de realizar los administradores locales completa de ellos.

Control de acceso basado en roles en Windows Admin Center funciona mediante la configuración de cada servidor administrado con un PowerShell [Just Enough Administration](https://aka.ms/jeadocs) punto de conexión.
Este punto de conexión define las funciones, incluyendo información sobre los aspectos del sistema de cada rol tiene permiso para administrar y qué usuarios están asignados al rol.
Cuando un usuario se conecta al punto de conexión restringido, se crea una cuenta de administrador local temporal para administrar el sistema en su nombre.
Esto garantiza que incluso las herramientas que no tienen su propio modelo de delegación todavía se pueden administrar con Windows Admin Center.
La cuenta temporal se quita automáticamente cuando el usuario deja de administrar la máquina a través de Windows Admin Center.

Cuando un usuario se conecta a un equipo configurado con control de acceso basado en roles, Windows Admin Center comprobará primero si son un administrador local.
Si lo son, recibirán la experiencia completa de Windows Admin Center sin restricciones.
En caso contrario, Windows Admin Center comprobará si el usuario pertenece a alguno de los roles predefinidos.
Se dice que un usuario tiene *acceso limitado* si pertenecen a un rol de Windows Admin Center pero que no son de administrador total.
Por último, si el usuario no es administrador ni un miembro de un rol, se le denegará el acceso para administrar la máquina.

Control de acceso basado en roles está disponible para las soluciones de clúster de conmutación por error y de administrador del servidor.

### <a name="available-roles"></a>Roles disponibles

Windows Admin Center admite los siguientes roles de usuario final:

Nombre de función | Uso previsto
----------|-------------
Administradores | Permite a los usuarios usar la mayoría de las características en Windows Admin Center sin concederles acceso a Escritorio remoto o PowerShell. Este rol es adecuado para escenarios de "servidor de salto" donde desea limitar los puntos de entrada de administración en un equipo.
Lectores | Permite a los usuarios ver información y la configuración en el servidor, pero no realizar cambios.
Administradores de Hyper-V | Permite a los usuarios realizar cambios en las máquinas virtuales de Hyper-V y conmutadores, pero limita otras características de acceso de solo lectura.

Las siguientes extensiones integradas tienen funcionalidad reducida cuando un usuario se conecta con acceso limitado:

- Archivos (sin carga de archivos o descarga)
- PowerShell (no disponible)
- Escritorio remoto (no disponible)
- Réplica de almacenamiento (no disponible)

En este momento, no se puede crear roles personalizados para su organización, pero puede elegir qué usuarios se les concede acceso a cada rol.

### <a name="preparing-for-role-based-access-control"></a>Preparación para el control de acceso basado en roles

Para aprovechar las cuentas locales temporales, cada máquina de destino debe configurarse para admitir el control de acceso basado en roles en Windows Admin Center.
El proceso de configuración incluye la instalación de los scripts de PowerShell y un punto de conexión de Just Enough Administration en la máquina con Desired State Configuration.

Si solo tiene unos pocos equipos, se puede aplicar fácilmente la configuración individualmente a cada equipo mediante la página de control de acceso basado en roles de Windows Admin Center.
Al establecer el control de acceso basado en roles en un equipo individual, se crean grupos de seguridad locales para controlar el acceso a cada rol.
Puede conceder acceso a los usuarios u otros grupos de seguridad al agregarlas como miembros de los grupos de seguridad de rol.

Para una implementación de toda la empresa en varios equipos, puede descargar el script de configuración de la puerta de enlace y distribuirla a los equipos con un servidor de extracción de Desired State Configuration, Azure Automation o las herramientas de administración preferido.
