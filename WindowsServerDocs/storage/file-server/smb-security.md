---
title: Mejoras de seguridad SMB
description: Obtener una explicación de la característica de cifrado de SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 831ca8266c3ec18ffb83227dcb2d39b3f953ad1a
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233531"
---
# <a name="smb-security-enhancements"></a>Mejoras de seguridad SMB

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

En este tema se explica las mejoras de seguridad SMB en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.

## <a name="smb-encryption"></a>Cifrado de SMB

Cifrado de SMB proporciona cifrado de extremo a otro de los datos SMB y protege los datos de interceptación repeticiones en redes que no se confía. Puede implementar el cifrado de SMB con el mínimo esfuerzo, pero puede requerir pequeños costos adicionales para especializados hardware o software. No tiene los requisitos de seguridad de protocolo de Internet (IPsec) o aceleradores de WAN. Cifrado de SMB pueden configurarse de forma de compartir por o para el servidor de archivos completo, y se puede habilitar para una variedad de escenarios donde datos atraviesan redes que no se confía.

>[!NOTE]
>Cifrado de SMB no se trata la seguridad en reposo, que normalmente se controla mediante el cifrado de unidad BitLocker.

Cifrado de SMB se deben considerar para cualquier escenario en el que deben protegerse frente a ataques de man-in-the-middle datos confidenciales. Los escenarios posibles son:

- Datos confidenciales de un trabajador de la información se mueven mediante el protocolo SMB. El cifrado de SMB ofrece una garantía de integridad y privacidad de extremo a extremo entre el servidor de archivos y el cliente, independientemente de las redes atravesados, por ejemplo, de área extensa (WAN) conexiones de red se mantienen por los proveedores que no son de Microsoft.
- SMB 3.0 permite a los servidores de archivos proporcionar almacenamiento continuamente disponible para aplicaciones de servidor, como SQL Server o Hyper-V. Habilitar el cifrado de SMB proporciona una oportunidad para proteger esa información desde cualquier ataques. Cifrado de SMB es más fácil de usar que las soluciones de hardware dedicado que son necesarias para la mayoría redes de área de almacenamiento (SAN).

>[!IMPORTANT]
>Tenga en cuenta que hay una notable de rendimiento operativo costo con cualquier protección de cifrado de extremo a extremo en comparación con no cifrada.

## <a name="enable-smb-encryption"></a>Habilitar el cifrado de SMB

Puede habilitar el cifrado de SMB para el servidor de archivos completo o sólo para recursos compartidos de archivos específicos. Utilice uno de los siguientes procedimientos para habilitar el cifrado de SMB:

### <a name="enable-smb-encryption-with-windows-powershell"></a>Habilitar el cifrado de SMB con Windows PowerShell

1. Para habilitar el cifrado de SMB para un recurso compartido de archivo individuales, escriba el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbShare –Name <sharename> -EncryptData $true
    ```
2. Para habilitar el cifrado de SMB para el servidor de archivos completo, escriba el siguiente script en el servidor:
    
    ```PowerShell
    Set-SmbServerConfiguration –EncryptData $true
    ```
3. Para crear un nuevo recurso compartido de archivos SMB con el cifrado de SMB habilitado, escriba el siguiente script:
    
    ```PowerShell
    New-SmbShare –Name <sharename> -Path <pathname> –EncryptData $true
    ```

### <a name="enable-smb-encryption-with-server-manager"></a>Habilitar el cifrado de SMB con el administrador del servidor

1. En el administrador del servidor, abra el **archivo y servicios de almacenamiento**.
2. Seleccione **los recursos compartidos** para abrir la página de administración de recursos compartidos.
3. Haga clic en el recurso compartido en el que desea habilitar el cifrado de SMB y, a continuación, seleccione **Propiedades**.
4. En la página **configuración** del recurso compartido, seleccione **cifrar el acceso a datos**. Acceso a este recurso compartido de archivos remoto se cifra.

### <a name="considerations-for-deploying-smb-encryption"></a>Consideraciones para la implementación de cifrado de SMB

De forma predeterminada, cuando está habilitado el cifrado de SMB para un recurso compartido de archivos o un servidor, se permiten sólo los clientes de SMB 3.0 para tener acceso a los recursos compartidos de archivo especificado. Esto exige la intención del Administrador de protección de los datos para todos los clientes que tienen acceso a los recursos compartidos. Sin embargo, en algunos casos, un administrador es posible que desee permitir el acceso sin cifrar para los clientes que no admiten 3.0 de SMB (por ejemplo, durante un período de transición cuando se usan versiones de sistema operativo cliente mixto). Para permitir el acceso sin cifrar para los clientes que no admiten SMB 3.0, escriba el siguiente script de Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –RejectUnencryptedAccess $false
```

La capacidad de la negociación del dialecto seguro que se describen en la siguiente sección impide que un ataque de man-in-the-middle degradar una conexión desde 3.0 de SMB a SMB 2.0 (que usaría acceso sin cifrar). Sin embargo, no impide una degradación a SMB 1.0, que también diese como resultado acceso sin cifrar. Para garantizar que los clientes SMB 3.0 siempre usen cifrado de SMB para tener acceso a los recursos compartidos de cifrado, debe deshabilitar al servidor SMB 1.0. (Para obtener instrucciones, vea la sección [deshabilitación de SMB 1.0](#disabling-smb-1.0)). Si el valor de **RejectUnencryptedAccess –** se deja en su valor predeterminado de **$true**, se permiten solo cifrado clientes SMB 3.0 para tener acceso a los recursos compartidos de archivo (también se rechazará los clientes SMB 1.0).

>[!NOTE]
>* Cifrado de SMB utiliza el estándar de cifrado avanzado (AES)-algoritmo de MCC para cifrar y descifrar los datos. AES-MCC también proporciona la validación de la integridad de datos (firma) para recursos compartidos de archivos cifrados, independientemente de la configuración de firma de SMB. Si desea habilitar la firma sin cifrado de SMB, puede seguir para ello. Para obtener más información, vea [Los conceptos básicos de la firma de SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).
>* Puede encontrar problemas al intentar tener acceso el recurso compartido de archivos o el servidor si su organización usa dispositivos de aceleración de área extensa (WAN) de la red.
>* Con una configuración predeterminada (donde no hay ningún acceso sin cifrar permitido a recursos compartidos de archivos cifrados), si los clientes que no admiten SMB 3.0 intenta tener acceso a un recurso compartido de archivos cifrados, 1003 de identificador de evento se registra en el registro de eventos de Microsoft-Windows-SmbServer/Operational , y el cliente recibe un mensaje de error **acceso denegado** .
>* Cifrado de SMB y el sistema de archivos cifrados (EFS) en el sistema de archivos NTFS no están relacionadas, y no requiere cifrado de SMB o no dependen de utilizando EFS.
>* Cifrado de SMB y el cifrado de unidad BitLocker no están relacionadas, y no requiere cifrado de SMB o no dependen de utilizando el cifrado de unidad BitLocker.

## <a name="secure-dialect-negotiation"></a>Proteger la negociación del dialecto

SMB 3.0 es capaz de detectar los ataques de man-in-the-middle que intentan degradar el protocolo SMB 2.0 o 3.0 de SMB o las capacidades que negocian el cliente y el servidor. Cuando se detecta este tipo de ataque por el cliente o el servidor, la conexión se desconecta y 1005 de identificador de evento se registra en el registro de eventos de Microsoft-Windows-SmbServer/Operational. Seguro dialecto negociación no se puede detectar o evitar la degradación de SMB 2.0 o 3.0 para SMB 1.0. Por este motivo y para aprovechar todas las capacidades de cifrado de SMB, recomendamos encarecidamente que deshabilite al servidor SMB 1.0. Para obtener más información, vea [deshabilitar SMB 1.0](#disabling-smb-1.0).

La capacidad de negociación dialecto segura que se describe en la siguiente sección impide que un ataque de man-in-the-middle degradar una conexión desde 3 de SMB a 2 de SMB (que usaría acceso sin cifrar); Sin embargo, no impide la degradación 1 de SMB, lo que daría como resultado también en access sin cifrar. Para obtener más información sobre posibles problemas con versiones anteriores que no sean Windows implementaciones de SMB, vea la [Microsoft Knowledge Base](http://support.microsoft.com/kb/2686098).

## <a name="new-signing-algorithm"></a>Nuevo algoritmo de firma

SMB 3.0 usa un algoritmo de cifrado más reciente para firmar: cifrado estándar avanzado (AES) - cifrado - según el código de autenticación de mensajes (CMAC). SMB 2.0 usa el algoritmo de cifrado HMAC-SHA256 más antiguos. AES-CMAC y AES-MCC pueden acelerar considerablemente el cifrado de datos en las CPU más moderno que tener compatibilidad con el conjunto de instrucciones AES. Para obtener más información, vea [Los conceptos básicos de la firma de SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

## <a name="disabling-smb-10"></a>Deshabilitación de SMB 1.0

El servicio de explorador de equipo heredado y características del protocolo de administración remota en SMB 1.0 ahora son independientes y pueden eliminarse. Estas características aún están habilitadas de forma predeterminada, pero si no tienen clientes SMB más antiguos, como los equipos que ejecutan Windows Server 2003 o Windows XP, puede quitar las características de SMB 1.0 para aumentar la seguridad y reducir potencialmente la aplicación de revisiones.

>[!NOTE]
>SMB 2.0 se introdujo en Windows Server 2008 y Windows Vista. Los clientes más antiguos, como los equipos que ejecutan Windows Server 2003 o Windows XP, no admiten SMB 2.0; y, por lo tanto, no podrán tener acceso a recursos compartidos de archivos o recursos compartidos de impresión si el servidor SMB 1.0 está deshabilitado. Además, algunos clientes que no sean Microsoft SMB no podrá tener acceso a recursos compartidos de archivos de SMB 2.0 o imprimir recursos compartidos (por ejemplo, impresoras con funcionalidad "examen para compartir").

Antes de comenzar la deshabilitación de SMB 1.0, que necesitará para averiguar si los clientes SMB están actualmente conectados al servidor que ejecuta SMB 1.0. Para ello, escriba el siguiente cmdlet de Windows PowerShell:

```PowerShell
Get-SmbSession | Select Dialect,ClientComputerName,ClientUserName | ? Dialect -lt 2
```

>[!NOTE]
>Debe ejecutar este script varias veces en el transcurso de una semana (varias veces al día) para crear una pista de auditoría. También podría ejecutar como una tarea programada.

Para deshabilitar SMB 1.0, escriba el siguiente script de Windows PowerShell:

```PowerShell
Set-SmbServerConfiguration –EnableSMB1Protocol $false
```

>[!NOTE]
>Si una conexión de cliente SMB se deniega porque se ha deshabilitado el servidor que ejecuta SMB 1.0, identificador de evento 1001 se registrarán en el registro de eventos de Microsoft-Windows-SmbServer/Operational.

## <a name="more-information"></a>Más información

A continuación presentamos algunos recursos adicionales sobre SMB y tecnologías relacionadas en Windows Server 2012.

- [Bloque de mensajes del servidor](file-server-smb-overview.md)
- [Almacenamiento en Windows Server](../storage.md)
- [Servidor de archivos de escalado horizontal para datos de la aplicación](../../failover-clustering/sofs-overview.md)