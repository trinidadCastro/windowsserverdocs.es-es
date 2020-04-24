---
title: Resolver códigos de error de activación de Windows
description: Aprenda a solucionar problemas de códigos de error de activación.
ms.topic: article
ms.date: 9/18/2019
ms.technology: server-general
author: kaushika-msft
ms.author: kaushika-msft; v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 50c50353316db4288f01893125ecd651db63cbb7
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826358"
---
# <a name="resolve-windows-activation-error-codes"></a>Resolver códigos de error de activación de Windows

> **Usuarios particulares**  
> Este artículo está pensado para que lo usen los agentes de soporte técnico y profesionales de TI. Si quiere obtener más información acerca de los mensajes de error de activación de Windows, consulte [Obtener ayuda con los errores de activación de Windows](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors).  

En este artículo se proporciona información de solución de problemas que le ayudará a responder a los mensajes de error que puede recibir al intentar usar una clave de activación múltiple (MAK) o el Servicio de administración de claves (KMS) para realizar la activación de volumen en uno o varios equipos basados en Windows. Busque el código de error en la tabla siguiente y, a continuación, seleccione el vínculo para ver más información sobre el código de error y cómo resolverlo.

Para obtener más información sobre la activación de volumen, vea [Plan para la activación de volumen](https://docs.microsoft.com/windows/deployment/volume-activation/plan-for-volume-activation-client).

Para obtener más información acerca de la activación de volumen para las versiones actuales y recientes de Windows, consulte [Activación de volumen [cliente]](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-windows-10).

Para obtener más información acerca de la activación de volumen para versiones anteriores de Windows, consulte KB 929712, [información de activación de volumen para Windows Vista, Windows Server 2008, Windows Server 2008 R2 y Windows 7](https://support.microsoft.com/help/929712/volume-activation-information-for-windows-vista-windows-server-2008-wi).

## <a name="diagnostic-tool"></a>Herramienta de diagnóstico

El Asistente de soporte y recuperación (SaRA) de Microsoft simplifican la solución de problemas de activación de KMS de Windows. Descargue la herramienta de diagnóstico desde [aquí](https://aka.ms/SaRA-WindowsActivation).

Esta herramienta intentará activar Windows. Si devuelve un código de error de activación, la herramienta mostrará soluciones de destino para los códigos de error conocidos.

Se admiten los códigos de error siguientes: 0xC004F038, 0xC004F039, 0xC004F041, 0xC004F074, 0xC004C008.

## <a name="summary-of-error-codes"></a>Resumen de códigos de error

|Código de error |Mensaje de error |Tipo de&nbsp;activación|
|-----------|--------------|----------------|
|[0x8004FE21](#0x8004fe21-this-computer-is-not-running-genuine-windows) |Este equipo no está ejecutando Windows original.  |MAK<br />Cliente KMS |
|[0x80070005](#0x80070005-access-denied) |Acceso denegado: la acción solicitada requiere privilegios elevados. |MAK<br />Cliente KMS<br />Host de KMS |
|[0x8007007b](#0x8007007b-dns-name-does-not-exist) |0x8007007b El nombre DNS no existe. |Cliente KMS |
|[0x80070490](#0x80070490-the-product-key-you-entered-didnt-work) |La clave de producto que escribió no funcionó. Compruebe la clave de producto y vuelva a intentarlo o especifique otra diferente. |MAK |
|[0x800706BA](#0x800706ba-the-rpc-server-is-unavailable) |El servidor RPC no está disponible. |Cliente KMS |
|[0x8007232A](#0x8007232a-dns-server-failure) |Error de servidor DNS.  |Host de KMS  |
|[0x8007232A](#0x8007232b-dns-name-does-not-exist) |El nombre DNS no existe. |Cliente KMS |
|[0x8007251D](#0x8007251d-no-records-found-for-dns-query) |No se ha encontrado ningún registro para la consulta de DNS. |Cliente KMS |
|[0x80092328](#0x80092328-dns-name-does-not-exist) |El nombre DNS no existe.  |Cliente KMS |
|[0xC004B100](#0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated) |El servidor de activación determinó que no se pudo activar el equipo. |MAK |
|[0xC004C001](#0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid) |El servidor de activación determinó que la clave de producto especificada no es válida. |MAK|
|[0xC004C003](#0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked) |El servidor de activación determinó que la clave de producto especificada está bloqueada. |MAK |
|[0xC004C008](#0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used) |El servidor de activación determinó que la clave de producto especificada no se pudo usar. |KMS |
|[0xC004C020](#0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit) |El servidor de activación notificó que la Clave de activación múltiple superó su límite. |MAK |
|[0xC004C021](#0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded) |El servidor de activación notificó que la Clave de activación múltiple superó su límite de extensión. |MAK |
|[0xC004F009](#0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired) |El servicio de protección de software informó de que el período de gracia expiró. |MAK |
|[0xC004F00F](#0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance) |El servidor de licencias de software notificó que el enlace del identificador de hardware supera el nivel de tolerancia. |MAK<br />Cliente KMS<br />Host de KMS |
|[0xC004F014](#0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available) |El servicio de protección de software informó de que la clave de producto no está disponible. |MAK<br />Cliente KMS |
|[0xC004F02C](#0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect) |El servicio de protección de software informó de que el formato de los datos de activación sin conexión es incorrecto. |MAK<br />Cliente KMS |
|[0xC004F035](#0xc004f035-invalid-volume-license-key) |El servicio de protección de software informó de que no se pudo activar el equipo con una clave de producto de licencia por volumen. |Cliente KMS<br />Host de KMS |
|[0xC004F038](#0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient) |El servicio de protección de software notificó que no se pudo activar el equipo. El número devuelto desde el Servicio de administración de claves (KMS) no es suficiente. Póngase en contacto con el administrador del sistema. |Cliente KMS |
|[0xC004F039](#0xc004f039-the-key-management-service-kms-is-not-enabled) |El servicio de protección de software notificó que no se pudo activar el equipo. El Servicio de administración de claves (KMS) no está habilitado. |Cliente KMS |
|[0xC004F041](#0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated) |El servicio de protección de software determinó que el servidor de administración de claves (KMS) no está activado. KMS tiene que activarse.  |Cliente KMS |
|[0xC004F042](#0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used) |El servicio de protección de software determinó que el Servicio de administración de claves (KMS) no se pudo usar. |Cliente KMS |
|[0xC004F050](#0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid) |El servicio de protección de software informó de que la clave de producto no es válida. |MAK<br />KMS<br />Cliente KMS |
|[0xC004F051](#0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked) |El servicio de protección de software informó de que la clave de producto está bloqueada. |MAK<br />KMS |
|[0xC004F064](#0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired) |El servicio de protección de software notificó que expiró el período de gracia para software no original. |MAK |
|[0xC004F065](#0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period) |El servicio de protección de software notificó que la aplicación se está ejecutando dentro del período de gracia válido para software no original. |MAK<br />Cliente KMS |
|[0xC004F06C](#0xc004f06c-the-request-timestamp-is-invalid) |El servicio de protección de software notificó que no se pudo activar el equipo. El Servicio de administración de claves (KMS) determinó que la marca de tiempo de la solicitud no es válida.  |Cliente KMS |
|[0xC004F074](#0xc004f074-no-key-management-service-kms-could-be-contacted) |El servicio de protección de software notificó que no se pudo activar el equipo. No se pudo establecer contacto con ningún Servicio de administración de claves (KMS). Consulte el registro de eventos de la aplicación para obtener más información.  |Cliente KMS |

## <a name="causes-and-resolutions"></a>Causas y resoluciones

### <a name="0x8004fe21-this-computer-is-not-running-genuine-windows"></a>0x8004FE21: este equipo no está ejecutando Windows original  

#### <a name="possible-cause"></a>Causa posible

Este problema puede ocurrir por varios motivos. El motivo más probable es que se hayan instalado paquetes de idioma (MUI) en equipos que ejecutan ediciones de Windows que no disponen de licencia para incluir paquetes de idiomas adicionales.  

> [!NOTE]
> Este problema no es necesariamente una señal de manipulación. Algunas aplicaciones pueden incluir compatibilidad con varios idiomas aunque esa edición de Windows no tenga licencia para esos paquetes de idiomas).  

Este problema puede producirse también si algún tipo de malware ha modificado Windows para permitir que se instalen características adicionales. Este problema puede producirse también si algunos archivos del sistema están dañados.  

#### <a name="resolution"></a>Solución

Para resolver este problema, debe volver a instalar el sistema operativo.  

### <a name="0x80070005-access-denied"></a>0x80070005: acceso denegado

El texto completo de este mensaje de error es similar al siguiente:

> Acceso denegado: la acción solicitada requiere privilegios elevados.

#### <a name="possible-cause"></a>Causa posible

El Control de cuentas de usuario (UAC) impide que se ejecuten los procesos de activación en una ventana del símbolo del sistema sin permisos elevados.  

#### <a name="resolution"></a>Solución

Ejecute **slmgr.vbs** desde un símbolo del sistema con permisos elevados. Para ello, en el **menú Inicio**, haga clic con el botón derecho en **cmd.exe** y, a continuación, seleccione **Ejecutar como administrador**.  

### <a name="0x8007007b-dns-name-does-not-exist"></a>0x8007007b: el nombre DNS no existe

#### <a name="possible-cause"></a>Causa posible

Este problema puede ocurrir si el cliente de KMS no puede encontrar los registros de recursos SRV de KMS en DNS.  

#### <a name="resolution"></a>Solución

Para obtener más información acerca de la solución de problemas relacionados con DNS, consulte [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80070490-the-product-key-you-entered-didnt-work"></a>0x80070490: la clave de producto que escribió no funcionó

El texto completo de este error es similar al siguiente:
> La clave de producto que escribió no funcionó. Compruebe la clave de producto y vuelva a intentarlo o especifique otra diferente.  

#### <a name="possible-cause"></a>Causa posible

Este problema ocurre porque se especificó una clave MAK no válida, o debido a un problema conocido en Windows Server 2019.  

#### <a name="resolution"></a>Solución

Para solucionar este problema y activar el equipo, ejecute **slmgr -ipk <5x5 key>** en un símbolo del sistema con permisos elevados.

### <a name="0x800706ba-the-rpc-server-is-unavailable"></a>0x800706BA: el servidor RPC no está disponible

#### <a name="possible-cause"></a>Causa posible

Los ajustes del firewall no están configurados en el host de KMS o los registros SRV de DNS son obsoletos.  

#### <a name="resolution"></a>Solución

En el host de KMS, asegúrese de que la excepción de firewall esté habilitada para el Servicio de administración de claves (puerto TCP 1688).

Asegúrese de que los registros SRV de DNS apunten a un host de KMS válido. 

Resuelva los problemas relacionados con las conexiones de red.  

Para obtener más información acerca de la solución de problemas relacionados con DNS, consulte [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007232a-dns-server-failure"></a>0x8007232A: error de servidor DNS

#### <a name="possible-cause"></a>Causa posible

El sistema presenta problemas de red o DNS.

#### <a name="resolution"></a>Solución

Resuelva los problemas de red y DNS.  

### <a name="0x8007232b-dns-name-does-not-exist"></a>0x8007232B: el nombre DNS no existe

#### <a name="possible-cause"></a>Causa posible

El cliente de KMS no encuentra los registros de recursos del servidor de KMS (RR de SRV) en DNS.  

#### <a name="resolution"></a>Solución

Verifique que se haya instalado un host de KMS y que la publicación de DNS se encuentre habilitada (valor predeterminado). Si DNS no está disponible, apunte el cliente de KMS al host de KMS por medio de **slmgr.vbs /skms <*kms_host_name*>** .  

Si no tiene ningún host de KMS, obtenga e instale una clave MAK. A continuación, active el sistema.

Para obtener más información acerca de la solución de problemas relacionados con DNS, consulte [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007251d-no-records-found-for-dns-query"></a>0x8007251D: no se encontró ningún registro para la consulta de DNS

#### <a name="possible-cause"></a>Causa posible

El cliente de KMS no encuentra los registros SRV de KMS en DNS.

#### <a name="resolution"></a>Solución

Resuelva los problemas relacionados con las conexiones de red y DNS. Para obtener más información acerca de cómo solucionar problemas relacionados con DNS, consulte [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80092328-dns-name-does-not-exist"></a>0x80092328: el nombre DNS no existe

#### <a name="possible-cause"></a>Causa posible

Este problema puede ocurrir si el cliente de KMS no puede encontrar los registros de recursos SRV de KMS en DNS.

#### <a name="resolution"></a>Solución

Para obtener más información acerca de la solución de problemas relacionados con DNS, consulte [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated"></a>0xC004B100: el servidor de activación determinó que no se pudo activar el equipo

#### <a name="possible-cause"></a>Causa posible

MAK no es compatible.  

#### <a name="resolution"></a>Solución

Para solucionar este problema, verifique que la clave MAK que utiliza es la que proporcionó Microsoft. Para verificar que la MAK sea válida, póngase en contacto con los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid"></a>0xC004C001: el servidor de activación determinó que la clave de producto especificada no es válida

#### <a name="possible-cause"></a>Causa posible

La clave MAK que especificó no es válida.

#### <a name="resolution"></a>Solución

Verifique que la clave sea la de tipo MAK que proporcionó Microsoft. Para obtener más ayuda, póngase en contacto con los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked"></a>0xC004C003: el servidor de activación determinó que la clave de producto especificada está bloqueada

#### <a name="possible-cause"></a>Causa posible

La MAK se encuentra bloqueada en el servidor de activación.

#### <a name="resolution"></a>Solución

Para obtener una nueva clave MAK, póngase en contacto con los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers). Una vez obtenga la nueva clave MAK, intente instalar y activar Windows de nuevo.  

### <a name="0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used"></a>0xC004C008: el servidor de activación determinó que la clave de producto especificada no se pudo usar

#### <a name="possible-cause"></a>Causa posible

Se superó el límite de activaciones de la clave KMS. Una clave de host de KMS se puede activar hasta 10 veces en seis equipos diferentes.  

#### <a name="resolution"></a>Solución

Si necesita más activaciones, póngase en contacto los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).  

### <a name="0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit"></a>0xC004C020: el servidor de activación notificó que la clave de activación múltiple superó su límite

#### <a name="possible-cause"></a>Causa posible

La clave MAK ha superado su límite de activación. Por naturaleza, las claves MAK se pueden activar un número limitado de veces.

#### <a name="resolution"></a>Solución

Si necesita más activaciones, póngase en contacto los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded"></a>0xC004C021: el servidor de activación notificó que la clave de activación múltiple superó su límite de extensión

#### <a name="possible-cause"></a>Causa posible

La clave MAK ha superado su límite de activación. Por naturaleza, las claves MAK se activan un número limitado de veces.

#### <a name="resolution"></a>Solución

Si necesita más activaciones, póngase en contacto los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired"></a>0xC004F009: el servicio de protección de software informó de que el período de gracia expiró

#### <a name="possible-cause"></a>Causa posible

El periodo de gracia finalizó antes de que se activara el sistema. El sistema se encuentra ahora en estado de Notificaciones.  

#### <a name="resolution"></a>Solución

Para obtener ayuda, póngase en contacto con los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance"></a>0xC004F00F: el servidor de licencias de software notificó que el enlace del identificador de hardware supera el nivel de tolerancia

#### <a name="possible-cause"></a>Causa posible

El hardware ha cambiado o se actualizaron los controladores del sistema.  

#### <a name="resolution"></a>Solución

Si utiliza la activación de MAK, use la activación en línea o por teléfono para reactivar el sistema durante el período de gracia de fuera de tolerancia (OOT).  

Si usa la activación de KMS, reinicie Windows o ejecute **slmgr.vbs /ato**.

### <a name="0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available"></a>0xC004F014: el servicio de protección de software informó de que la clave de producto no está disponible

#### <a name="possible-cause"></a>Causa posible

No hay claves de producto instaladas en el sistema.  

#### <a name="resolution"></a>Solución

Si usa la activación de MAK, instale una clave de producto de MAK. 

Si usa la activación de KMS, compruebe el archivo Pid.txt (que se encuentra en los medios de instalación de la carpeta \sources) para obtener una clave de configuración de KMS. Instale la clave.

### <a name="0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect"></a>0xC004F02C: el servicio de protección de software informó de que el formato de los datos de activación sin conexión es incorrecto

#### <a name="possible-cause"></a>Causa posible

El sistema ha detectado que los datos introducidos durante la activación telefónica no son válidos.  

#### <a name="resolution"></a>Solución

Verifique que el CID se haya introducido correctamente.  

### <a name="0xc004f035-invalid-volume-license-key"></a>0xC004F035: la clave de licencia por volumen no es válida

El texto completo de este mensaje de error es similar al siguiente:

> Error: La clave de licencia por volumen no es válida. Para activarla debe cambiar la clave de producto a una clave de activación múltiple (MAK) o clave comercial. Debe tener una licencia de sistema operativo susceptible de actualización Y una licencia por volumen de actualización de Windows 7, o una licencia completa de Windows 7 de una fuente comercial. CUALQUIER OTRA INSTALACIÓN DE ESTE SOFTWARE CONSTITUYE UNA INFRACCIÓN DEL CONTRATO Y DE LAS LEYES DE COPYRIGHT APLICABLES.  

El texto del error es correcto, pero resulta ambiguo. Este error indica que el equipo no tiene un marcador de Windows en su BIOS que lo identifique como sistema OEM que ejecuta una edición certificada de Windows. Esta información es necesaria para la activación del cliente de KMS. El significado más específico de este código es "Error: La clave de licencia por volumen no es válida".

#### <a name="possible-cause"></a>Causa posible

Las licencias de las ediciones por volumen de Windows 7 solo se conceden con fines de actualización. Microsoft no admite la instalación de un sistema operativo por volumen en un equipo que no tenga un sistema operativo susceptible de actualización instalado.  

#### <a name="resolution"></a>Solución

Para activar, debes seguir uno de los procedimientos siguientes:

- Cambia la clave de producto a una clave de activación múltiple (MAK) o clave comercial. Debe tener una licencia de sistema operativo susceptible de actualización Y una licencia por volumen de actualización de Windows 7, o una licencia completa de Windows 7 de una fuente comercial.
  > [!NOTE]
  > Si recibes el error 0x80072ee2 al intentar activar, usa en su lugar el método de activación por teléfono siguiente.
- Para activar por teléfono, sigue estos pasos:
   1. Ejecuta **slmgr /dti** y, luego, registra el valor del identificador de instalación. </li>
   1. Ponte en contacto con los [Centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers) e indica el identificador de instalación para recibir un identificador de confirmación.</li>
   1. Para activar mediante el identificador de confirmación, ejecuta **slmgr /atp &lt;id. de confirmación&gt;** .

### <a name="0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient"></a>0xC004F038: el número devuelto desde el Servicio de administración de claves (KMS) no es suficiente

El texto completo de este mensaje de error es similar al siguiente:

> El servicio de protección de software notificó que no se pudo activar el equipo. El número devuelto desde el Servicio de administración de claves (KMS) no es suficiente. Póngase en contacto con el administrador del sistema.  

#### <a name="possible-cause"></a>Causa posible

El número que figura en el host de KMS no es suficientemente alto. En Windows Server, el número de KMS debe ser mayor o igual que 5. En Windows (cliente), el número de KMS debe ser mayor o igual que 25.  

#### <a name="resolution"></a>Solución
Para poder usar KMS para activar Windows, debe tener más equipos en el grupo de KMS. Para obtener el número actual en el host de KMS, ejecute **Slmgr.vbs /dli**.  

### <a name="0xc004f039-the-key-management-service-kms-is-not-enabled"></a>0xC004F039: el Servicio de administración de claves (KMS) no está habilitado

El texto completo de este mensaje de error es similar al siguiente:

> El servicio de protección de software notificó que no se pudo activar el equipo. El Servicio de administración de claves (KMS) no está habilitado.  

#### <a name="possible-cause"></a>Causa posible

KMS no respondió a la solicitud de KMS.

#### <a name="resolution"></a>Solución

Resuelva el problema de conexión de red entre el host de KMS y el cliente. Asegúrese de que el puerto TCP 1688 (predeterminado) no se encuentre bloqueado por un firewall u otro tipo de filtro.

### <a name="0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated"></a>0xC004F041: el servicio de protección de software determinó que el servidor de administración de claves (KMS) no está activado

El texto completo de este mensaje de error es similar al siguiente:

> El servicio de protección de software determinó que el servidor de administración de claves (KMS) no está activado. KMS tiene que activarse.  

#### <a name="possible-cause"></a>Causa posible

El host de KMS no está activado.  

#### <a name="resolution"></a>Solución

Active el host de KMS mediante la opción en línea o por teléfono.  

### <a name="0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used"></a>0xC004F042: el servicio de protección de software determinó que el Servicio de administración de claves (KMS) especificado no se puede usar

#### <a name="possible-cause"></a>Causa posible

Este error se produce si el cliente de KMS contacta con un host de KMS que no puede activar el software del cliente. Esto puede ser habitual, por ejemplo, en entornos mixtos que contienen hosts de KMS específicos de aplicaciones y de sistemas operativos.  

#### <a name="resolution"></a>Solución

Asegúrese de que si usa hosts de KMS específicos para activar determinados sistemas operativos o aplicaciones, los clientes de KMS se conectan a los hosts correctos.

### <a name="0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid"></a>0xC004F050: el servicio de protección de software informó de que la clave de producto no es válida

#### <a name="possible-cause"></a>Causa posible

Puede deberse a un error de escritura en la clave KMS o por la escritura en una clave Beta de una versión de lanzamiento del sistema operativo.  

#### <a name="resolution"></a>Solución

Instale la clave KMS apropiada en la versión correspondiente de Windows. Revise la ortografía. Si la clave se copia y se pega, asegúrese de que los guiones largos no se hayan sustituido por guiones en la clave.  

### <a name="0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked"></a>0xC004F051: el servicio de protección de software informó de que la clave de producto está bloqueada

#### <a name="possible-cause"></a>Causa posible

El servidor de activación determinó que Microsoft bloqueó la clave de producto.  

#### <a name="resolution"></a>Solución

Obtenga una clave MAK o KMS nueva, instálela en el sistema y actívela.

### <a name="0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired"></a>0xC004F064: el servicio de protección de software notificó que expiró el período de gracia para software no original

#### <a name="possible-cause"></a>Causa posible

Las herramientas de activación de Windows han determinado que el sistema no es original.  

#### <a name="resolution"></a>Solución

Para obtener ayuda, póngase en contacto con los [centros de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers).

### <a name="0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period"></a>0xC004F065: el servicio de protección de software notificó que la aplicación se está ejecutando dentro del período de gracia válido para software no original

#### <a name="possible-cause"></a>Causa posible

Las herramientas de activación de Windows han determinado que el sistema no es original. El sistema continuará ejecutándose durante el período de gracia válido para software no original.  

#### <a name="resolution"></a>Solución

Obtenga e instale una clave de producto original y active el sistema durante el período de gracia. En caso contrario, el sistema entrará en el estado de Notificaciones al final del período de gracia.

### <a name="0xc004f06c-the-request-timestamp-is-invalid"></a>0xC004F06C: la marca de tiempo de la solicitud no es válida

El texto completo de este mensaje de error es similar al siguiente:

> El servicio de protección de software notificó que no se pudo activar el equipo. El Servicio de administración de claves (KMS) determinó que la marca de tiempo de la solicitud no es válida.  

#### <a name="possible-cause"></a>Causa posible

La configuración horaria del sistema del equipo cliente difiere demasiado de la del host de KMS. La sincronización horaria es importante para la protección del sistema y de la red por diversos motivos.  

#### <a name="resolution"></a>Solución

Para solucionar este problema, cambie la hora del sistema en el cliente para sincronizarla con la del KMS. Le recomendamos que use un origen de hora NTP (Protocolo de tiempo de redes) o Active Directory Domain Services para la sincronización horaria. En este error interviene la hora UTP y es independiente de la selección de la zona horaria.  

### <a name="0xc004f074-no-key-management-service-kms-could-be-contacted"></a>0xC004F074: no se pudo establecer contacto con ningún Servicio de administración de claves (KMS)

El texto completo de este mensaje de error es similar al siguiente:

> El servicio de protección de software notificó que no se pudo activar el equipo. No se pudo establecer contacto con ningún Servicio de administración de claves (KMS). Consulte el registro de eventos de la aplicación para obtener más información.  

#### <a name="possible-cause"></a>Causa posible

Todos los sistemas de host de KMS devolvieron un error.  

#### <a name="resolution"></a>Solución

En el registro de eventos de aplicación, identifique cada evento que tenga el id. de evento 12288 y esté asociado al intento de activación. Solucione los errores de estos eventos.

Para obtener más información acerca de la solución de problemas relacionados con DNS, consulte [Procedimientos habituales de solución de problemas de KMS y DNS](common-troubleshooting-procedures-kms-dns.md).  
