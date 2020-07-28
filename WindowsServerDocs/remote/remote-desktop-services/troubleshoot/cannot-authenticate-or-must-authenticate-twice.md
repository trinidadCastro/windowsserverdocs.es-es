---
title: El usuario no se puede autenticar o debe autenticarse dos veces
description: Solución de un problema en el que el usuario no se puede autenticar o debe autenticarse dos veces cuando se inicia una conexión a Escritorio remoto.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 65bf81d80d5cfd5566d66005728cd3677f131085
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963227"
---
# <a name="user-cant-authenticate-or-must-authenticate-twice"></a>El usuario no se puede autenticar o debe autenticarse dos veces

En este artículo se describen varios problemas que pueden afectar a la autenticación del usuario.

## <a name="access-denied-restricted-type-of-logon"></a>Acceso denegado, tipo de inicio de sesión restringido

En esta situación, se deniega el acceso a un usuario de Windows 10 que intenta conectarse a equipos con Windows 10 o Windows Server 2016 con el siguiente mensaje:

> Conexión a Escritorio remoto:  
> El administrador del sistema ha restringido el tipo de inicio de sesión (red o interactiva) que pueda usar. Para obtener ayuda, ponte en contacto con el administrador del sistema o con el personal de soporte técnico.

Este problema se produce cuando se requiere autenticación a nivel de red (NLA) para las conexiones RDP y el usuario no es miembro del grupo**Usuarios de escritorio remoto**. También puede producirse si no se ha asignado el grupo**Usuarios de escritorio remoto** al derecho de usuario **Tener acceso a este equipo desde la red**.

Para resolver este problema, sigue uno de estos pasos:

  - [Modificación de la asignación de permisos del usuario o la pertenencia a un grupo del usuario](#modify-the-users-group-membership-or-user-rights-assignment)
  - Desactiva NLA (no se recomienda).
  - Usa clientes de escritorio remoto que no sean de Windows 10. Por ejemplo, los clientes de Windows 7 no tienen este problema.

### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modificación de la asignación de permisos del usuario o la pertenencia a un grupo del usuario

Si este problema afecta a un solo usuario, la solución más sencilla consiste en agregar el usuario al grupo **Usuarios de escritorio remoto**.

Si el usuario ya es miembro de este grupo (o si varios miembros del grupo tienen el mismo problema), compruebe la configuración de los permisos del usuario en el equipo remoto con Windows 10 o Windows Server 2016.

1. Abre el Editor de objetos de directiva de grupo (GPE) y conéctese a la directiva local del equipo remoto.
2. Vete a **Configuración del equipo \\Configuración de Windows \\Configuración de seguridad\\Directivas locales\\** , haz clic con el botón derecho en **Tener acceso a este equipo desde la red** y luego selecciona **Propiedades**.
3. Consulta en la lista de usuarios y grupos **Usuarios de escritorio remoto** (o un grupo primario).
4. Si en la lista no se incluye **Usuarios de Escritorio Remoto** o un grupo primario como **Todos**, tendrás que agregarlo a la lista. Si tienes más de un equipo en la implementación, usa un objeto de directiva de grupo.  
    Por ejemplo, la pertenencia predeterminada de **Tener acceso a este equipo desde la red** incluye **Todos**. Si la implementación usa un objeto de directiva de grupo para quitar **Todos**, es posible que debas restaurar el acceso mediante la actualización del objeto de directiva de grupo para agregar **Usuarios de escritorio remoto**.

## <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Acceso denegado, se ha denegado una llamada remota a la base de datos de SAM

Es muy probable que este comportamiento se produzca si el controlador de dominio utiliza Windows Server 2016, o cualquier versión o posterior, y los usuarios intentan conectarse mediante una aplicación de conexión personalizada. En concreto, se denegará el acceso a las aplicaciones que acceden a la información del perfil del usuario en Active Directory.

Este comportamiento es consecuencia de un cambio en Windows. En Windows Server 2012 R2 y versiones anteriores, cuando un usuario inicia sesión en un escritorio remoto, el Administrador de conexiones remotas (RCM) se pone en contacto con el controlador de dominio (DC) para consultar las configuraciones que son específicas de Escritorio remoto en el objeto de usuario en Active Directory Domain Services (AD DS). Esta información se muestra en la pestaña Perfil de Servicios de Escritorio remoto de las propiedades del objeto de un usuario en el complemento MMC Usuarios y equipos de Active Directory.

A partir de Windows Server 2016, RCM ya no consulta el objeto del usuario en AD DS. Si necesitas que RCM consulte AD DS porque usas los atributos de Servicios de Escritorio remoto, debes habilitar la consulta de forma manual.

> [!IMPORTANT]  
> Sigue meticulosamente los pasos que se describen en esta sección. Pueden producirse problemas graves si modifica el Registro de manera incorrecta. Antes de modificarlo, [haz una copia de seguridad del registro para restaurarlo](https://support.microsoft.com/help/322756), por si se produjeran problemas.

Para habilitar el comportamiento heredado de RCM en un servidor host de sesión de Escritorio remoto, configura las siguientes entradas del Registro y reinicia el servicio **Servicios de Escritorio remoto**:  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Nombre: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valor: **1** (decimal)

Para habilitar el comportamiento heredado de RCM en cualquier servidor que no sea el servidor host de sesión de Escritorio remoto, configura estas entradas del Registro y la siguiente entrada del Registro adicional (y reinicia el servicio):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Para más información acerca de este comportamiento, consulta el KB 3200967 [Cambios en el Administrador de conexiones remotas en Windows Server](https://support.microsoft.com/help/3200967/changes-to-remote-connection-manager-in-windows-server).

## <a name="user-cant-sign-in-using-a-smart-card"></a>El usuario no puede iniciar sesión con una tarjeta inteligente

En esta sección se abordan tres escenarios comunes en los que un usuario no puede iniciar sesión en un escritorio remoto con una tarjeta inteligente.

### <a name="cant-sign-in-with-a-smart-card-in-a-branch-office-with-a-read-only-domain-controller-rodc"></a>No se puede iniciar sesión con una tarjeta inteligente en una sucursal con un controlador de dominio de solo lectura (RODC)

Este problema se produce en las implementaciones que incluyen un servidor RDSH en un sitio de sucursal que usa un RODC. El servidor RDSH está hospedado en el dominio raíz. Los usuarios del sitio de rama pertenecen a un dominio secundario y usan tarjetas inteligentes para la autenticación. El RODC está configurado para almacenar en la caché las contraseñas de usuario (el RODC pertenece al **Grupo de replicación de contraseña RODC permitida**). Cuando los usuarios intentan iniciar sesión en sesiones del servidor RDSH, reciben mensajes, como "El inicio de sesión intentado no es válido. Esto se debe a un nombre de usuario o a información de autenticación incorrectos."

Este problema se debe a la forma en que el controlador de dominio raíz y el RDOC administran el cifrado de credenciales de usuario. El controlador de dominio raíz usa una clave de cifrado para cifrar las credenciales y el RODC proporciona al cliente la clave de descifrado. Cuando un usuario recibe el error "no válido", significa que las dos claves no coinciden.

Para solucionar este problema, sigue uno de estos pasos:

- Para cambiar la topología de controlador de dominio, desactiva el almacenamiento en caché de contraseñas en el RODC, o bien implementa un controlador de dominio grabable en el sitio de la sucursal.
- Mueve el servidor RDSH al mismo dominio secundario en el que se encuentran los usuarios.
- Permite a los usuarios iniciar sesión sin tarjetas inteligentes.

Recuerda que todas estas soluciones requieren compromisos en el nivel de rendimiento o de seguridad.

### <a name="user-cant-sign-in-to-a-windows-server-2008-sp2-computer-using-a-smart-card"></a>El usuario no puede iniciar sesión en un equipo con Windows Server 2008 SP2 con una tarjeta inteligente

Este problema se produce cuando los usuarios inician sesión en un equipo con Windows Server 2008 SP2 que se ha actualizado con KB4093227 (2018.4B). Cuando los usuarios intentan iniciar sesión mediante una tarjeta inteligente, se les deniega el acceso con mensajes como "No se encontraron certificados válidos. Comprueba que la tarjeta se ha insertado correctamente y encaja perfectamente." Al mismo tiempo, el equipo con Windows Server registra el evento de programa "Error al recuperar un certificado digital de la tarjeta inteligente insertada. Firma no válida."

Para resolver este problema, actualiza el equipo con Windows Server con el nuevo lanzamiento de 2018.06 B de 4093227 KB, [Descripción de la actualización de seguridad para la vulnerabilidad de denegación de servicio de Protocolo de escritorio remoto (RDP) en Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

### <a name="cant-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>No se puede mantener la sesión iniciada con una tarjeta inteligente y el servicio Servicios de Escritorio remoto se bloquea

Este problema se produce cuando los usuarios inician sesión en un equipo con Windows o Windows Server que se ha actualizado con KB 4056446. Al principio, el usuario puede iniciar sesión en el sistema mediante el uso de una tarjeta inteligente, pero luego recibe el mensaje de error "SCARD\_E\_NO\_SERVICE". Es posible que el equipo remoto deje de responder.

Para solucionar este problema, reinicia el equipo remoto.

Para resolver este problema, actualice el equipo remoto con la corrección adecuada:

  - Windows Server 2008 SP2: KB 4090928, [Windows leaks handles in the lsm.exe process and smart card applications may display "SCARD\_E\_NO\_SERVICE" errors](https://support.microsoft.com/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe) (Windows pierde controladores en el proceso de lsm.exe y las aplicaciones de la tarjeta inteligente pueden mostrar errores "SCARD_E_NO_SERVICE")
  - Windows Server 2012 R2: KB 4103724, [17 de mayo de 2018: KB4103724 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 y Windows 10, versión 1607: KB 4103720, [17 de mayo de 2018: KB4103720 (compilación de sistema operativo 14393.2273)](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)

## <a name="if-the-remote-pc-is-locked-the-user-needs-to-enter-a-password-twice"></a>Si se bloquea el equipo remoto, el usuario debe escribir la contraseña dos veces

Este problema se puede producir cuando un usuario intenta conectarse a un escritorio remoto en el que se ejecuta Windows 10, versión 1709, en una implementación en la que las conexiones de RDP no requieren NLA. En estas condiciones, si el escritorio remoto se ha bloqueado, el usuario debe escribir sus credenciales dos veces al conectarse.

Para resolver este problema, actualiza el equipo con Windows 10, versión 1709, con KB 4343893, [30 de agosto de 2018: KB4343893 (compilación del sistema operativo 16299.637)](https://support.microsoft.com/help/4343893/windows-10-update-kb4343893).

## <a name="user-cant-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>El usuario no puede iniciar sesión y recibe los mensajes "error de autenticación" y "corrección de oráculo de cifrado de CredSSP"

Cuando los usuarios intentan iniciar sesión con cualquier versión de Windows a partir de Windows Vista SP2 y posteriores, o Windows Server 2008 SP2 y posteriores, se les deniega el acceso y reciben mensajes como estos:

```
An authentication error has occurred. The function requested is not supported.
...
This could be due to CredSSP encryption oracle remediation
...
```

"Corrección de oráculo de cifrado de CredSSP" es un conjunto de actualizaciones de seguridad que se lanzó en marzo, abril y mayo de 2018. CredSSP es un proveedor de autenticación que procesa solicitudes de autenticación de otras aplicaciones. Tanto la actualización "3B", de 13 de marzo de 2018, como las actualizaciones posteriores solucionaban una vulnerabilidad de seguridad en la que un atacante podría transmitir credenciales de usuario para ejecutar código en el sistema de destino.

Las actualizaciones iniciales agregaban compatibilidad con un nuevo objeto de directiva de grupo, Corrección de oráculo de cifrado, que tiene los siguientes valores posibles:

  - Vulnerable: Las aplicaciones cliente que usan CredSSP pueden revertirse a versiones no seguras, pero este comportamiento expone los escritorios remotos a ataques. Los servicios que usan CredSSP aceptan clientes que no se hayan actualizado.
  - Mitigado: las aplicaciones cliente que usan CredSSP no se pueden revertir a versiones no seguras, pero los servicios que usan CredSSP aceptan clientes que no se han actualizado.
  - Forzar clientes actualizados: las aplicaciones cliente que usan CredSSP no se pueden revertir a versiones no seguras y los servicios que usan CredSSP no aceptarán clientes sin revisiones. 
    > [!NOTE]
    > Este valor no se debe implementar hasta que todos los hosts remotos admitan la versión más reciente.

En la actualización del 8 de mayo de 2018 se ha cambiado el valor predeterminado de Corrección de oráculo de cifrado de Vulnerable a Mitigado. Con este cambio, los clientes de Escritorio remoto que tengan las actualizaciones no se pueden conectar a servidores que no las tengan (ni a servidores actualizados que no se hayan reiniciado). Para más información sobre las actualizaciones de CredSSP, consulta [KB 4093492](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver este problema, actualiza y reinicia todos los sistemas. Para ver una lista completa de las actualizaciones y más información acerca de las vulnerabilidades, consulta [CVE-2018-0886 | CredSSP Remote Code Execution Vulnerability](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2018-0886) (CVE-2018-0886 | Vulnerabilidad de ejecución de código remoto de CredSSP).

Para solucionar este problema hasta que se completen las actualizaciones, consulta en KB 4093492 los tipos de conexiones permitidos. Si no hay alternativas viables, puedes considerar la posibilidad de usar uno de los métodos siguientes:

- Para los equipos cliente afectados, vuelve a establecer la directiva Corrección de oráculo de cifrado en **Vulnerable**.
- Modifica las siguientes directivas en la carpeta de directivas de grupo **Configuración del equipo\\Plantillas administrativas\\Componentes de Windows\\Servicios de Escritorio remoto\\Host de sesión de Escritorio remoto\\ Seguridad**:  
  - **Requerir el uso de un nivel de seguridad específico para conexiones remotas (RDP)** : establécelo en **Habilitado** y selecciona **RDP**.
  - **Requerir la autenticación de usuario para las conexiones remotas mediante Autenticación a nivel de red**: establécelo en **Deshabilitado**.
    > [!IMPORTANT]  
    > El cambio de estas directivas de grupo reduce la seguridad de la implementación. Se recomienda que solo las uses temporalmente, si las necesitas.

Para más información acerca de cómo trabajar con directivas de grupo, consulta [Modificación de un GPO de bloqueo](rdp-error-general-troubleshooting.md#modifying-a-blocking-gpo).

## <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Después de actualizar los equipos cliente, algunos usuarios necesitan iniciar sesión dos veces

Cuando los usuarios inician sesión en Escritorio remoto con un equipo que ejecuta Windows 7 o Windows 10, versión 1709, verán inmediatamente un segundo mensaje de inicio de sesión. Este problema se produce si el equipo cliente tiene las actualizaciones siguientes:

  - Windows 7: KB 4103718, [8 de mayo de 2018: KB4103718 (paquete acumulativo mensual)](https://support.microsoft.com/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de mayo de 2018: KB4103727 (compilación de sistema operativo 16299.431)](https://support.microsoft.com/help/4103727/windows-10-update-kb4103727)

Para resolver este problema, asegúrate de que los equipos a los que desean conectarse los usuarios (así como los servidores RDSH o RDVI) están totalmente actualizados en junio de 2018. Esto incluye las siguientes actualizaciones:

  - Windows Server 2016: KB 4284880, [12 de junio de 2018: KB4284880 (compilación del sistema operativo 14393.2312)](https://support.microsoft.com/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junio de 2018: KB4284815 (paquete acumulativo mensual)](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junio de 2018: KB4284855 (paquete acumulativo mensual)](https://support.microsoft.com/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junio de 2018: KB4284826 (paquete acumulativo mensual)](https://support.microsoft.com/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [Descripción de la actualización de seguridad para la vulnerabilidad de ejecución de código remoto de CredSSP de Windows Server 2008, Windows Embedded POSReady 2009 y Windows Embedded Standard 2009: 13 de marzo de 2018](https://support.microsoft.com/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

## <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Se deniega el acceso a los usuarios en una implementación que usa Credential Guard remoto con varios agentes de conexión a Escritorio remoto

Este problema se produce en las implementaciones de alta disponibilidad que utilizan dos, o más, agentes de conexión a Escritorio remoto, siempre que Credential Guard remoto de Windows Defender esté en uso. Los usuarios no pueden iniciar sesión en escritorios remotos.

Este problema se produce porque Credential Guard remoto usa Kerberos para la autenticación y restringe NTLM. Pero en una configuración de alta disponibilidad con equilibrio de carga, los agentes de conexión a Escritorio remoto no admiten operaciones de Kerberos.

Si necesitas usar una configuración de alta disponibilidad con agentes de conexión a Escritorio remoto con equilibrio de carga, puedes solucionar el problema mediante la deshabilitación de Credential Guard remoto. Para más información acerca de cómo administrar Credential Guard remoto de Windows Defender, consulta [Protección de credenciales de escritorio remoto con Credential Guard remoto de Windows Defender](/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).
