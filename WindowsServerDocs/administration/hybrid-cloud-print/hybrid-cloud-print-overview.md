---
title: Información general de la impresión de Windows Server híbrida en la nube
description: Impresión de en la nube híbrida permite a los profesionales de TI admitir los requisitos de impresión para BYOD o dominio unido dispositivos.
ms.prod: w10
ms.mktglfcycl: manage
ms.sitesec: library
ms.pagetype: store
ms.technology: server-general
author: TrudyHa
ms.author: TrudyHa
ms.date: 10/16/2017
ms.openlocfilehash: faa9fde857a9a4ee3f7c03f682b3dbced0340417
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878836"
---
# <a name="windows-server-hybrid-cloud-print-overview"></a>Información general de la impresión de Windows Server híbrida en la nube

**Se aplica a**
-   Windows Server 2016

## <a name="what-is-hybrid-cloud-print"></a>¿Qué es la impresión de nube híbrida?
**Impresión de nube híbrida** es una característica nueva de Windows Server 2016 disponibles a través de **características a petición**. Permite a los profesionales de TI admitir los requisitos de impresión para las personas que traen sus propios dispositivos o utilizan dispositivos Unidos a Azure Active Directory. Esto incluye dispositivos móviles como teléfonos Windows, equipos portátiles o tabletas con Windows 10 o Windows Mobile. Proporciona compatibilidad con la impresión desde cualquier lugar, las personas tienen acceso a Internet.

Los administradores de TI, **Print de nube híbrida** proporciona acceso de usuario seguras a las impresoras locales mediante el uso de autenticación multifactor de Azure para validar el acceso de usuario. Inicio de sesión en función único (SSO) simplifica la experiencia del usuario. **Impresión de nube híbrida** se basa en Windows **servidor de impresión** rol, que ofrece a los profesionales de TI una experiencia similar a la administración de impresoras y seguridad de acceso del usuario.

**Impresión de en la nube híbrida** permite a los usuarios de su organización para imprimir desde los dispositivos que usan para completar su trabajo - incluso cuando están fuera de su departamento de soporte técnico o al área de trabajo.

**Impresión de nube híbrida** es compatible con Windows 10 Creators Update y Windows 10 S.
 
## <a name="feature-summary"></a>Resumen de características
**Impresión de nube híbrida** consta de dos componentes principales de servidor: **Detección** service, y **Windows Print** service.
- **Detección** extremo de servicio que se ejecuta en un servicio IIS que admiten el estándar del sector de Mopria Alliance para la detección de impresora en la nube.
- **Impresión de Windows** admiten estándar imprimir Protocolo Internet (IPP) para asegurar el sistema operativo de cliente más amplio de punto de conexión de servicio que se ejecuta en un servicio IIS que admiten la industria.

## <a name="deployment"></a>Implementación
**Impresión de nube híbrida** es compatible con un par de opciones de implementación diferentes dependiendo de dónde la organización requiere autenticación del usuario. Este es el aspecto que podría una implementación:

![Diagrama que muestra una representación gráfica de la solución de impresión de la nube híbrida](../media/hybrid-cloud-print/wshcp-deployment-options.png)

*Diagrama de solución de impresión en la nube híbrida*

El diagrama se muestra:
- **Impresión de nube híbrida** mediante Azure Active Directory como proveedor de identidades de usuario. 
- **Impresión de Windows** service y **detección** los extremos de servicio están registrados con Azure Active Directory para habilitar el dispositivo de cliente recuperar el token de autenticación de usuario necesarios para utilizar con estos servicios. 
- Servicio MDM como **Microsoft Intune**, aprovisiona el dispositivo de cliente con las directivas necesarias para conectar Azure Active Directory para **Windows Print** servicio y **detección**servicio.

Esta tabla tiene más información sobre los elementos en el diagrama.  

| Elemento | Descripción |
| ------- | ----------- |
| Azure Active Directory  | Proporciona y controla la funcionalidad de identidad y autorización de usuario |
| Active Directory        | Proporciona y controla la funcionalidad de identidad y autorización de usuario |
| Azure AD Connect  | Sincroniza las credenciales de usuario entre Azure AD y AD local. |
| Servicio de MDM (Intune) | Proporciona funcionalidad de aprovisionamiento de directiva de dispositivo para garantizar que el dispositivo de cliente (dispositivo BYOD) cumple con las directivas corporativas. |
| Proxy de Azure AD | Proporciona una conexión de larga duración que se establece a través del firewall en Azure para permitir el tráfico de aplicación configurado específica fluya desde Internet a la red corporativa. |
| Azure Web App | El núcleo de la solución de impresión de nube híbrida. Proporciona los extremos web necesario para detectar impresoras y enviar contenido de impresión para dispositivos Unidos al dominio. |
| Dispositivos BYOD / servidor de impresión Windows / impresora | Se trata como-es. Ningún cambio en la funcionalidad de la implementación. |

Hay dos formas de instalar **Print de nube híbrida**:
- **Las características a petición** -consulte [configurar características a petición en Windows Server](https://docs.microsoft.com/windows-server/administration/server-manager/configure-features-on-demand-in-windows-server) para más información sobre cómo agregar y quitar archivos de roles y características. 
- **Configuración de Windows Server 2016** -los administradores pueden ir a **configuración** -> **aplicaciones** -> **administrar características opcionales**  ->  **Agregar una característica** y busque las características en el paquete a petición 
- Comandos de PowerShell - ventana de administrador en unas PowerShell, ejecutarlos estos comandos:

```PowerShell
    Install-Module -Name PublishCloudPrinter
    Import-Module PublishCloudPrinter
    ```
