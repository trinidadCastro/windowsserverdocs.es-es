---
title: Opciones de acceso de usuario con Windows Admin Center
description: Opciones de acceso de usuario y proveedores de identidades con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "71356917"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opciones de acceso de usuario con Windows Admin Center

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Cuando se implementa en Windows Server, Windows Admin Center proporciona un punto de administración centralizado para el entorno del servidor. Al controlar el acceso a Windows Admin Center, puedes mejorar la seguridad de tu entorno de administración.

## <a name="gateway-access-roles"></a>Roles de acceso a la puerta de enlace

Windows Admin Center define dos roles para el acceso al servicio de puerta de enlace: usuarios de puerta de enlace y administradores de puerta de enlace.

> [!NOTE]
> El acceso a la puerta de enlace no implica el acceso a los servidores de destino visibles para la puerta de enlace. Para administrar un servidor de destino, un usuario debe conectarse con credenciales que tengan privilegios administrativos en el servidor de destino.

Los **usuarios de puerta de enlace** pueden conectarse al servicio de puerta de enlace de Windows Admin Center para administrar servidores mediante la puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación que se usa para autenticarse en la puerta de enlace.

Los **administradores de puerta de enlace** pueden configurar quién obtiene acceso y cómo se autentican los usuarios en la puerta de enlace.

>[!NOTE]
> Si no hay ningún grupo de acceso definido en Windows Admin Center, los roles reflejarán el acceso de la cuenta de Windows al servidor de puerta de enlace. 

[Configura el acceso de usuario y de administrador de la puerta de enlace en Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opciones de proveedores de identidades

Los administradores de la puerta de enlace pueden elegir cualquiera de las siguientes opciones:

 - [Active Directory o grupos de máquinas locales](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory como proveedor de identidades de Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticación de tarjeta inteligente

Si usas Active Directory o grupos de máquinas locales como proveedor de identidades, para exigir la autenticación de tarjeta inteligente, solicita a los usuarios que acceden a Windows Admin Center que sean miembros de grupos de seguridad basados en tarjetas inteligentes adicionales. [Configura la autenticación de tarjeta inteligente en Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Acceso condicional y autenticación multifactor

Al requerir la autenticación de Azure AD para la puerta de enlace, puedes utilizar las características de seguridad adicionales, como el acceso condicional y la autenticación multifactor que proporciona Azure AD. [Más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Control de acceso basado en roles

De forma predeterminada, los usuarios requieren privilegios de administrador local completos en las máquinas que quieren administrar mediante Windows Admin Center.
Esto les permite conectarse a la máquina de forma remota y garantiza que tienen permisos suficientes para ver y modificar la configuración del sistema.
Sin embargo, es posible que algunos usuarios no necesiten acceso sin restricciones a la máquina para realizar sus trabajos.
Puedes usar el **control de acceso basado en rol** en Windows Admin Center para proporcionar a estos usuarios acceso limitado a la máquina, en lugar de hacerles administradores locales completos.

El control de acceso basado en rol de Windows Admin Center funciona configurando cada servidor administrado con un punto de conexión [Just Enough Administration](https://aka.ms/jeadocs) de PowerShell.
Este punto de conexión define los roles, incluidos los aspectos del sistema que cada rol puede administrar y los usuarios que están asignados al rol.
Cuando un usuario se conecta al punto de conexión restringido, se crea una cuenta de administrador local temporal para administrar el sistema en su nombre.
Esto garantiza que incluso las herramientas que no tienen su propio modelo de delegación se puedan administrar con Windows Admin Center.
La cuenta temporal se quita automáticamente cuando el usuario deja de administrar la máquina a través de Windows Admin Center.

Cuando un usuario se conecta a una máquina configurada con el control de acceso basado en rol, Windows Admin Center comprueba primero si es un administrador local.
Si es así, recibirá la experiencia de Windows Admin Center completa sin restricciones.
De lo contrario, Windows Admin Center comprobará si el usuario pertenece a alguno de los roles predefinidos.
Se dice que un usuario tiene *acceso limitado* si pertenece a un rol del centro de Windows Admin Center, pero no es un administrador total.
Por último, si el usuario no es administrador ni miembro de un rol, se le denegará el acceso para administrar la máquina.

El control de acceso basado en rol está disponible para las soluciones Administrador del servidor y Clúster de conmutación por error.

### <a name="available-roles"></a>Roles disponibles

Windows Admin Center admite los siguientes roles de usuario final:

Nombre de función | Uso previsto
----------|-------------
Administradores | Permite a los usuarios usar la mayoría de las características de Windows Admin Center sin concederles acceso al Escritorio remoto ni a PowerShell. Este rol es adecuado para los escenarios de "servidor de salto" si se quieren limitar los puntos de entrada de administración en una máquina.
Lectores | Permite a los usuarios ver la información y la configuración en el servidor, pero no realizar cambios.
Administradores de Hyper-V | Permite a los usuarios realizar cambios en las máquinas virtuales de Hyper-V y en los modificadores, pero limita otras características al acceso de solo lectura.

Las siguientes extensiones integradas tienen una funcionalidad reducida cuando un usuario se conecta con acceso limitado:

- Archivos (sin carga ni descarga de archivos)
- PowerShell (no disponible)
- Escritorio remoto (no disponible)
- Réplica de almacenamiento (no disponible)

En este momento, no puedes crear roles personalizados para tu organización, pero puedes elegir qué usuarios obtendrán acceso a cada rol.

### <a name="preparing-for-role-based-access-control"></a>Preparación del control de acceso basado en rol

Para utilizar las cuentas locales temporales, cada máquina de destino debe configurarse para admitir el control de acceso basado en rol en Windows Admin Center.
El proceso de configuración implica la instalación de scripts de PowerShell y un punto de conexión Just Enough Administration en la máquina mediante Desired State Configuration.

Si solo tienes unos pocos equipos, puedes aplicar fácilmente la configuración de forma individual a cada uno mediante la página de control de acceso basado en rol en Windows Admin Center.
Cuando configuras el control de acceso basado en rol en un equipo individual, se crean grupos de seguridad locales para controlar el acceso a cada rol.
Para conceder acceso a usuarios u otros grupos de seguridad, les puedes agregar como miembros de los grupos de seguridad del rol.

En el caso de una implementación de toda la empresa en varias máquinas, puedes descargar el script de configuración desde la puerta de enlace y distribuirlo a los equipos mediante un servidor de extracción de Desired State Configuration, Azure Automation o tus herramientas de administración preferidas.
