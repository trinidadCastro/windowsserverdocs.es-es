---
title: Los clientes no se pueden conectar y reciben el error "No hay licencias disponibles"
description: Solución del error "No hay licencias disponibles" con la conexión a Escritorio remoto
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 540c368812866655452115d8928a915a07cacc97
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529975"
---
# <a name="clients-cant-connect-and-see-no-licenses-available-error"></a>Los clientes no se pueden conectar y ven el error "No hay licencias disponibles"

Esta situación se aplica a las implementaciones que incluyen un servidor RDSH y un servidor de administración de licencias de Escritorio remoto.

Primero, identifica qué comportamiento ven los usuarios:

- La sesión se ha desconectado porque no hay licencias disponibles o no hay ningún servidor de licencias disponible.
- Se ha denegado el acceso debido a un error de seguridad.

Inicia sesión en el host de sesión de Escritorio remoto como administrador de dominio y abre la herramienta Diagnóstico de licencias de Escritorio remoto. Busca mensajes como el siguiente:

  - El período de gracia del servidor remoto ha expirado en el servidor host de sesión de Escritorio remoto, pero no se ha configurado el servidor host de sesión de Escritorio remoto con los servidores de licencias. Las conexiones al servidor host de sesión de Escritorio remoto se denegarán, salvo que haya un servidor de licencias configurado para el servidor host de sesión de Escritorio remoto.
  - El servidor de licencias \<nombre de equipo\> no está disponible. Esto se puede deber a problemas de conectividad de la red, el servicio Administración de licencias de Escritorio remoto se detiene en el servidor de licencias o que la Administración de licencias de Escritorio remoto no esté disponible.

Estos problemas tienden a asociarse con los siguientes mensajes de usuario:

  - La sesión remota se desconectó porque no hay licencias de acceso de cliente de Escritorio remoto disponibles para este equipo.
  - La sesión remota se desconectó porque no hay servidores de licencias de Escritorio remoto disponibles proporcionar una licencia.

En este caso, [configure el servicio Administración de licencias de Escritorio remoto](#configure-the-rd-licensing-service).

Si la herramienta Diagnóstico de licencias de Escritorio remoto muestra otros problemas, como "El componente de protocolo RDP X.224 detectó un error en el flujo de protocolos y ha desconectado el cliente", es posible que haya un problema que afecta a los certificados de licencia. Dichos problemas tienden a asociarse con mensajes al usuario como los siguientes:

Debido a un error de seguridad, el cliente no se pudo conectar al servidor de Terminal Server. Cuando te hayas asegurado de que has iniciado sesión la red, intenta volver a conectarte al servidor.

En este caso, [actualiza las claves del Registro del certificado X509](#refresh-the-x509-certificate-registry-keys).

## <a name="configure-the-rd-licensing-service"></a>Configuración del servicio Administración de licencias de Escritorio remoto

El siguiente procedimiento usa el Administrador de servidores para realizar los cambios en la configuración. Para obtener información sobre cómo configurar y usar el Administrador de servidores, consulta [Administrador de servidores](../../../administration/server-manager/server-manager.md).

1. Abre el **Administrador de servidores** y navega a **Servicios de Escritorio remoto**.
2. En **Información general sobre la implementación** , selecciona **Tareas**y, después, selecciona **Edit Deployment Properties** (Editar propiedades de implementación).
3. Selecciona **Administración de licencias de Escritorio remoto** y después el modo de licencia adecuado para la implementación (**Por dispositivo** o **Por usuario**).
4. Escribe el nombre de dominio completo (FQDN) del servidor de licencias de RD y, después, selecciona **Agregar**.
5. Si tienes más de un servidor de licencias de RD, repite el paso 4 en cada uno de ellos. 
    ![Opciones de configuración del servidor de licencias de RD en el Administrador de servidores.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

## <a name="refresh-the-x509-certificate-registry-keys"></a>Actualización de las claves del Registro del certificado X509

> [!IMPORTANT]  
> Sigue detenidamente las instrucciones de esta sección. Se pueden producir problemas graves si el Registro se modifica de forma incorrecta. Antes de empezar a modificar el Registro, [haz una copia de seguridad del Registro](https://support.microsoft.com/help/322756) para poder restaurarlo en caso de que se produzca algún error.

Para resolver este problema, haz una copia de seguridad y, después, elimina las claves del Registro de certificado X509, reinicia el equipo y reactiva el servidor de licencias de RD. Sigue estos pasos.

> [!NOTE]
> Realiza el procedimiento siguiente en cada uno de los servidores RDSH.

Aquí se muestra cómo reactivar el Servidor de Administración de licencias de Escritorio remoto:

1. Abre el Editor del Registro y vete a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. En el menú Registro, selecciona **Exportar archivo del Registro**.
3. Escribe **exported- Certificate** en el cuadro **Nombre de archivo** cuadro y, después, selecciona **Guardar**.
4. Haz clic en cada uno de los siguientes valores, selecciona **Eliminar**y, después, selecciona **Sí** para comprobar la eliminación:  
      - **Certificado**
      - **Certificado X509**
      - **Identificador de certificado X509**
      - **Certificado X509 2**
5. Sal del Editor del Registro y reinicia el servidor RDSH.