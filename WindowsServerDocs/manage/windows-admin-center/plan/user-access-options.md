---
title: Opciones de acceso de usuario con el centro de administración de Windows
description: Opciones de acceso de usuario y proveedores de identidades con el centro de administración de Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356917"
---
# <a name="user-access-options-with-windows-admin-center"></a>Opciones de acceso de usuario con el centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Cuando se implementa en Windows Server, el centro de administración de Windows proporciona un punto centralizado de administración para el entorno del servidor. Al controlar el acceso al centro de administración de Windows, puede mejorar la seguridad de su entorno de administración.

## <a name="gateway-access-roles"></a>Roles de acceso de puerta de enlace

El centro de administración de Windows define dos roles para el acceso al servicio de puerta de enlace: usuarios de puerta de enlace y administradores de puerta de enlace.

> [!NOTE]
> El acceso a la puerta de enlace no implica el acceso a los servidores de destino visibles para la puerta de enlace. Para administrar un servidor de destino, un usuario debe conectarse con credenciales que tengan privilegios administrativos en el servidor de destino.

**Los usuarios de puerta de enlace** pueden conectarse al servicio de puerta de enlace del centro de administración de Windows para administrar los servidores a través de esa puerta de enlace, pero no pueden cambiar los permisos de acceso ni el mecanismo de autenticación que se usa para autenticarse en la puerta de enlace.

**Los administradores de puerta de enlace** pueden configurar quién obtiene el acceso y cómo se autenticarán los usuarios en la puerta de enlace.

>[!NOTE]
> Si no hay ningún grupo de acceso definido en el centro de administración de Windows, los roles reflejarán el acceso de la cuenta de Windows al servidor de puerta de enlace. 

[Configure el acceso de usuario y administrador de puerta de enlace en el centro de administración de Windows.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Opciones del proveedor de identidades

Los administradores de puerta de enlace pueden elegir cualquiera de las siguientes opciones:

 - [Active Directory o grupos de máquinas locales](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory como proveedor de identidades para el centro de administración de Windows](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Autenticación mediante tarjeta inteligente

Al usar Active Directory o grupos de máquinas locales como proveedor de identidades, puede exigir la autenticación de tarjeta inteligente al solicitar a los usuarios que tienen acceso al centro de administración de Windows que sean miembros de grupos de seguridad basados en tarjetas inteligentes adicionales. [Configurar la autenticación de tarjeta inteligente en el centro de administración de Windows.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Acceso condicional y autenticación multifactor

Al requerir la autenticación de Azure AD para la puerta de enlace, puede aprovechar características de seguridad adicionales, como el acceso condicional y la autenticación multifactor proporcionada por Azure AD. [Obtenga más información sobre cómo configurar el acceso condicional con Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Control de acceso basado en roles

De forma predeterminada, los usuarios requieren privilegios de administrador local completos en las máquinas que desean administrar mediante el centro de administración de Windows.
Esto les permite conectarse de forma remota al equipo y garantiza que tienen permisos suficientes para ver y modificar la configuración del sistema.
Sin embargo, es posible que algunos usuarios no necesiten acceso sin restricciones a la máquina para realizar sus trabajos.
Puede usar el **control de acceso basado en roles** en el centro de administración de Windows para proporcionar a estos usuarios acceso limitado a la máquina en lugar de hacerlo como administradores locales completos.

El control de acceso basado en roles del centro de administración de Windows funciona mediante la configuración de cada servidor administrado con un punto de conexión de [Administración suficiente](https://aka.ms/jeadocs) de PowerShell.
Este punto de conexión define los roles, incluidos los aspectos del sistema que cada rol puede administrar y los usuarios que están asignados al rol.
Cuando un usuario se conecta al punto de conexión restringido, se crea una cuenta de administrador local temporal para administrar el sistema en su nombre.
Esto garantiza que incluso las herramientas que no tienen su propio modelo de delegación todavía se pueden administrar con el centro de administración de Windows.
La cuenta temporal se quita automáticamente cuando el usuario deja de administrar la máquina a través del centro de administración de Windows.

Cuando un usuario se conecta a una máquina configurada con el control de acceso basado en roles, el centro de administración de Windows comprueba primero si es un administrador local.
Si es así, recibirán toda la experiencia del centro de administración de Windows sin restricciones.
De lo contrario, el centro de administración de Windows comprobará si el usuario pertenece a alguno de los roles predefinidos.
Se dice que un usuario tiene *acceso limitado* si pertenece a un rol del centro de administración de Windows, pero no es un administrador completo.
Por último, si el usuario no es un administrador ni un miembro de un rol, se le denegará el acceso para administrar el equipo.

El control de acceso basado en roles está disponible para las soluciones de clúster de conmutación por error y Administrador del servidor.

### <a name="available-roles"></a>Roles disponibles

El centro de administración de Windows admite los siguientes roles de usuario final:

Nombre de función | Uso previsto
----------|-------------
Administradores | Permite a los usuarios usar la mayoría de las características del centro de administración de Windows sin concederles acceso a Escritorio remoto o PowerShell. Este rol es adecuado para los escenarios de "servidor de salto" donde desea limitar los puntos de entrada de administración en un equipo.
Lectores | Permite a los usuarios ver información y configuraciones en el servidor, pero no realizar cambios.
Administradores de Hyper-V | Permite a los usuarios realizar cambios en las máquinas virtuales de Hyper-V y en los conmutadores, pero limita otras características al acceso de solo lectura.

Las siguientes extensiones integradas tienen una funcionalidad reducida cuando un usuario se conecta con acceso limitado:

- Archivos (sin carga o descarga de archivos)
- PowerShell (no disponible)
- Escritorio remoto (no disponible)
- Réplica de almacenamiento (no disponible)

En este momento, no se pueden crear roles personalizados para su organización, pero puede elegir a qué usuarios se les concede acceso a cada rol.

### <a name="preparing-for-role-based-access-control"></a>Preparación para el control de acceso basado en roles

Para aprovechar las cuentas locales temporales, cada máquina de destino debe configurarse para admitir el control de acceso basado en roles en el centro de administración de Windows.
El proceso de configuración implica la instalación de scripts de PowerShell y un punto de conexión de administración suficiente en el equipo mediante la configuración de estado deseado.

Si solo tiene unos pocos equipos, puede aplicar fácilmente la configuración de forma individual a cada equipo mediante la página control de acceso basado en roles del centro de administración de Windows.
Cuando se configura el control de acceso basado en roles en un equipo individual, se crean grupos de seguridad locales para controlar el acceso a cada rol.
Puede conceder acceso a usuarios u otros grupos de seguridad agregándolos como miembros de los grupos de seguridad de rol.

En el caso de una implementación de toda la empresa en varios equipos, puede descargar el script de configuración desde la puerta de enlace y distribuirlo a los equipos mediante un servidor de extracción de configuración de estado deseado, Azure Automation o sus herramientas de administración preferidas.
