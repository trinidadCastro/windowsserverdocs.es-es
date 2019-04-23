---
title: Administrar la seguridad de la capa de transporte (TLS)
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 8053a14a74797cccce4c441d41f1f1623ba0ad6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879446"
---
# <a name="manage-transport-layer-security-tls"></a>Administrar la seguridad de la capa de transporte (TLS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configurar TLS Cipher Suite orden

Diferentes versiones de Windows admiten diferentes conjuntos de cifrado TLS y orden de prioridad. Consulte [conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) para el orden predeterminado compatible con el Microsoft Schannel Provider en distintas versiones de Windows.

> [!NOTE] 
> También puede modificar la lista de conjuntos de cifrado mediante el uso de funciones de CNG, consulte [dando prioridad a los conjuntos de cifrado de Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) para obtener más información.

Los cambios realizados en el orden de conjuntos de cifrado TLS surtirá efecto en el siguiente arranque. Hasta el reinicio o apagado, el orden existente entrará en vigor.

> [!WARNING] 
> Actualizando la configuración del registro para el orden de prioridad predeterminado no se admite y puede restablecerse con las actualizaciones de mantenimiento. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configuración de orden de conjuntos de cifrado TLS mediante Directiva de grupo

Puede usar la configuración de directiva de grupo de orden de SSL cifrado Suite para configurar el orden predeterminado TLS cipher suite.

1.  En la consola de administración de directivas de grupo, vaya a **configuración del equipo** > **plantillas administrativas** > **redes**  >  **Opciones de configuración de SSL**.
2.  Haga doble clic en **orden de conjuntos de cifrado SSL**y, a continuación, haga clic en el **habilitado** opción.
3.  Haga clic en **conjuntos de cifrado SSL** cuadro y seleccione **seleccionar todo** en el menú emergente.

    ![Configuración de directiva de grupo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  Haga clic en el texto seleccionado y seleccione **copia** en el menú emergente.
5.  Pegue el texto en un editor de texto como notepad.exe y actualización con la nueva lista de orden del conjunto de cifrado.

    > [!NOTE]
    > La lista de pedidos TLS cipher suite debe estar en formato delimitado por estricta de comas. Cada cadena de cifrado suite finalizará con una coma (,) a la derecha de la misma. 

    > Además, la lista de conjuntos de cifrado se limita a 1023 caracteres.

6.  Reemplazar la lista en el **conjuntos de cifrado SSL** con la lista ordenada actualizada.
7.  Haga clic en **Aceptar** o en **Aplicar**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configuración de orden de conjuntos de cifrado TLS mediante MDM

El CSP de directivas de Windows 10 admite la configuración de los conjuntos de cifrado TLS. Consulte [criptografía/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) para obtener más información.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurar el orden de conjuntos de cifrado TLS mediante PowerShell Cmdlets de TLS

El módulo de PowerShell de TLS admite la obtención de la lista ordenada de conjuntos de cifrado TLS, un conjunto de cifrado de deshabilitar y habilitar un conjunto de cifrado. Consulte [TLS módulo](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) para obtener más información.

## <a name="configuring-tls-ecc-curve-order"></a>Configurar el orden de la curva de ECC TLS 

A partir de Windows 10 y Windows Server 2016, orden de la curva ECC puede configurarse independientemente de la orden de conjuntos de cifrado. Si la lista de sufijos de curva elíptica de tiene el orden de conjuntos de cifrado TLS, serán reemplazadas por el orden de prioridad de curva elíptica nuevo, cuando se habilita. Esto permite a las organizaciones usar un objeto de directiva de grupo para configurar las distintas versiones de Windows con el mismo orden de los conjuntos de cifrado.

> [!NOTE]
> Antes de Windows 10, cipher suite cadenas se anexan la curva elíptica para determinar la prioridad de la curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Administración de las curvas de ECC de Windows con CertUtil

A partir de Windows 10 y Windows Server 2016, Windows proporciona administración de parámetros de curva elíptica a través de la certutil.exe Utilidad línea de comandos. Parámetros de curva elíptica se almacenan en el bcryptprimitives.dll. Uso de certutil.exe, los administradores pueden agregar y quitar parámetros de curva hacia y desde Windows, respectivamente. Certutil.exe almacena los parámetros de curva de forma segura en el registro. Windows pueden empezar a usar los parámetros de curva por el nombre asociado con la curva.    

#### <a name="displaying-registered-curves"></a>Mostrar curvas registradas

Utilice el siguiente comando certutil.exe para mostrar una lista de las curvas registrado para el equipo actual.

```powershell
certutil.exe –displayEccCurve
```

![Curvas de presentación de certutil](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1 Certutil.exe de salida para mostrar la lista de las curvas registradas.*

#### <a name="adding-a-new-curve"></a>Agregar una nueva curva

Las organizaciones pueden crear y usar parámetros de curva investigados por otras entidades de confianza.  
Los administradores que deseen usar estas curvas nuevo en Windows deben agregar la curva.  
Use el siguiente comando certutil.exe para agregar una curva al equipo actual:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- El **curveName** argumento representa el nombre de la curva en la que se han agregado los parámetros de curva.
- El **curveParameters** argumento representa el nombre de archivo de un certificado que contiene los parámetros de las curvas que desee agregar.
- El **curveOid** argumento representa un nombre de archivo de un certificado que contiene el OID de los parámetros de curva que desea agregar (opcional).
- El **curveType** argumento representa un valor decimal de la curva con nombre desde el [Registry de curva con nombre CE](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (opcional).

![Certutil agregar curvas](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 adición de una curva mediante certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Eliminación de una curva previamente agregada

Los administradores pueden quitar una curva previamente agregada mediante el siguiente comando certutil.exe:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows no pueden usar una curva con nombre después de que un administrador quita la curva de equipo.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Administración de las curvas de ECC de Windows mediante directivas de grupo

Las organizaciones pueden distribuir los parámetros de curva a enterprise, unido al dominio, mediante la directiva de grupo y la extensión del registro de preferencias de directiva de grupo del equipo.  
El proceso para distribuir una curva es:

1.  En Windows 10 y Windows Server 2016, use **certutil.exe** para agregar una nueva curva con nombre registrada a Windows.
2.  Desde ese mismo equipo, abra la consola de administración de directivas de grupo (GPMC), cree un nuevo objeto de directiva de grupo y editarlo.
3.  Vaya a **configuración del equipo | Preferencias | Configuración de Windows | Registro**.  Haga clic en **registro**. Mantenga el mouse sobre **New** y seleccione **elemento de recopilación**. El nombre del elemento de colección para que coincida con el nombre de la curva. Se creará un elemento de recopilación del registro para cada clave del registro bajo *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurar la recopilación de registro del preferencias de directiva de grupo recién creado mediante la adición de un nuevo **elemento del registro** para cada valor del Registro enumerada en *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ ECCParameters\[curveName]*.
5.  Implemente el objeto de directiva de grupo que contiene el elemento de recopilación del registro de directiva de grupo para equipos Windows 10 y Windows Server 2016 que debe recibir las curvas con nombre nueva.

    ![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 Using Group Policy Preferences para distribuir las curvas*

## <a name="managing-tls-ecc-order"></a>Administrar el orden de TLS ECC

A partir de Windows 10 y Windows Server 2016, directiva de grupo de orden de la curva de ECC se puede usar la configuración de configurar el orden de curva de ECC TLS de forma predeterminada. Con ECC genérico y esta configuración, las organizaciones puede agregar sus propios confianza denominado curvas (que están aprobadas para su uso con TLS) para el sistema operativo y, a continuación, agregue esas curvas con nombre a la configuración de directiva de grupo de prioridad de curva para asegurarse de que se usan en futuras TLS protocolos de enlace. En el siguiente reinicio después de recibir la configuración de directiva se activan nuevas listas de prioridad de la curva.     

![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 administrar TLS curva prioridad con la directiva de grupo*


