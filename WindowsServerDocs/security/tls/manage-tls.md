---
title: Administrar la seguridad de la capa de transporte (TLS)
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: a4ac1ea5b0648dbb80f103c146ad3df23fc04ab7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402347"
---
# <a name="manage-transport-layer-security-tls"></a>Administrar la seguridad de la capa de transporte (TLS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>Configuración del orden del conjunto de cifrado TLS

Las distintas versiones de Windows admiten diferentes conjuntos de cifrado TLS y orden de prioridad. Vea [conjuntos de cifrado en TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) para el orden predeterminado admitido por el proveedor de Microsoft Schannel en diferentes versiones de Windows.

> [!NOTE] 
> También puede modificar la lista de conjuntos de cifrado mediante el uso de las funciones de CNG, consulte [priorización de conjuntos de cifrado de Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) para obtener más información.

Los cambios en el orden de los conjuntos de cifrado TLS surtirán efecto en el siguiente arranque. Hasta el reinicio o el apagado, se aplicará el orden existente.

> [!WARNING] 
> No se admite la actualización de la configuración del registro para el orden de prioridad predeterminado y se puede restablecer con actualizaciones de servicio. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configuración del orden del conjunto de cifrado TLS mediante directiva de grupo

Puede usar el orden de los conjuntos de cifrado SSL directiva de grupo configuración para configurar el orden predeterminado del conjunto de cifrado TLS.

1. En el Consola de administración de directivas de grupo, vaya a **configuración del equipo** > **Plantillas administrativas** > **redes** > opciones de **configuración de SSL**.
2. Haga doble clic en **orden de conjunto de cifrado SSL**y, a continuación, haga clic en la opción **habilitado** .
3. Haga clic con el botón secundario en el cuadro **conjuntos de cifrado SSL** y seleccione **seleccionar todo** en el menú emergente.

   ![Configuración de directiva de grupo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. Haga clic con el botón secundario en el texto seleccionado y seleccione **copiar** en el menú emergente.
5. Pegue el texto en un editor de texto, como Notepad. exe, y actualice con la nueva lista de orden de conjuntos de cifrado.

   > [!NOTE]
   > La lista de orden de conjuntos de cifrado TLS debe estar en formato delimitado por comas estricto. Cada cadena de conjunto de cifrado terminará con una coma (,) en el lado derecho de ella. 
   > 
   > Además, la lista de conjuntos de cifrado está limitada a 1.023 caracteres.

6. Reemplace la lista de los **conjuntos de cifrado SSL** por la lista ordenada actualizada.
7. Haga clic en **Aceptar** o en **Aplicar**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configuración del orden del conjunto de cifrado TLS mediante MDM

El CSP de directivas de Windows 10 admite la configuración de los conjuntos de cifrado TLS. Consulte [Cryptography/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) para obtener más información.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configuración del orden del conjunto de cifrado TLS mediante cmdlets de PowerShell de TLS

El módulo TLS PowerShell permite obtener la lista ordenada de conjuntos de cifrado TLS, deshabilitar un conjunto de cifrado y habilitar un conjunto de cifrado. Consulte [módulo TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) para obtener más información.

## <a name="configuring-tls-ecc-curve-order"></a>Configuración del orden de las curvas ECC de TLS 

A partir de Windows 10 & Windows Server 2016, el orden de las curvas ECC puede configurarse independientemente del orden de los conjuntos de cifrado. Si la lista de orden de conjuntos de cifrado TLS tiene sufijos de curva elíptica, se reemplazará por el nuevo orden de prioridad de curva elíptica cuando esté habilitada. Esto permite a las organizaciones usar un objeto de directiva de grupo para configurar diferentes versiones de Windows con el mismo orden de conjuntos de cifrado.

> [!NOTE]
> Antes de Windows 10, las cadenas de conjunto de cifrado se anexaban con la curva elíptica para determinar la prioridad de la curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Administrar curvas ECC de Windows mediante CertUtil

A partir de Windows 10 y Windows Server 2016, Windows proporciona administración de parámetros de curva elíptica a través de la utilidad de línea de comandos certutil. exe. Los parámetros de curva elíptica se almacenan en bcryptprimitives. dll. Con certutil. exe, los administradores pueden agregar y quitar parámetros de curva de Windows, respectivamente. Certutil. exe almacena los parámetros de curva de forma segura en el registro. Windows puede empezar a usar los parámetros de curva por el nombre asociado a la curva.    

#### <a name="displaying-registered-curves"></a>Mostrar curvas registradas

Use el siguiente comando certutil. exe para mostrar una lista de las curvas registradas para el equipo actual.

```powershell
certutil.exe –displayEccCurve
```

![Certutil Mostrar curvas](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1 resultado de certutil. exe para mostrar la lista de curvas registradas.*

#### <a name="adding-a-new-curve"></a>Agregar una nueva curva

Las organizaciones pueden crear y usar parámetros de curva investigados por otras entidades de confianza.  
Los administradores que deseen usar estas nuevas curvas en Windows deben agregar la curva.  
Use el siguiente comando certutil. exe para agregar una curva al equipo actual:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- El argumento **curveName** representa el nombre de la curva en la que se agregaron los parámetros de curva.
- El argumento **curveParameters** representa el nombre de archivo de un certificado que contiene los parámetros de las curvas que desea agregar.
- El argumento **curveOid** representa un nombre de archivo de un certificado que contiene el OID de los parámetros de curva que desea agregar (opcional).
- El argumento **curveType** representa un valor decimal de la curva con nombre del [registro de curva con nombre EC](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (opcional).

![Certutil-agregar curvas](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 agregar una curva mediante certutil. exe.*

#### <a name="removing-a-previously-added-curve"></a>Quitar una curva agregada previamente

Los administradores pueden quitar una curva agregada anteriormente mediante el siguiente comando certutil. exe:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows no puede usar una curva con nombre después de que un administrador Quite la curva del equipo.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Administrar curvas ECC de Windows mediante directiva de grupo

Las organizaciones pueden distribuir parámetros de curva a un equipo de empresa, unido a dominio, mediante directiva de grupo y la extensión del registro directiva de grupo Preferences.  
El proceso de distribución de una curva es el siguiente:

1.  En Windows 10 y Windows Server 2016, use **certutil. exe** para agregar una nueva curva con nombre registrada a Windows.
2.  En el mismo equipo, abra el Consola de administración de directivas de grupo (GPMC), cree un nuevo objeto directiva de grupo y edítelo.
3.  Vaya a **configuración del equipo | Preferencias | Configuración de Windows | Registro**.  Haga clic con el botón secundario en **registro**. Mantenga el mouse sobre **nuevo** y seleccione **elemento de recopilación**. Cambie el nombre del elemento de recopilación para que coincida con el nombre de la curva. Creará un elemento de colección de registro para cada clave del registro en *HKEY_LOCAL_MACHINE \currentcontrolset\control\cryptography\eccparameters*.
4.  Configure la colección del registro de preferencias directiva de grupo recién creada agregando un nuevo **elemento del registro** para cada valor del registro que aparece en *HKEY_LOCAL_MACHINE \currentcontrolset\control\cryptography\eccparameters\[curveName]* .
5.  Implemente el directiva de grupo objeto que contiene directiva de grupo elemento de colección del registro en equipos con Windows 10 y Windows Server 2016 que deben recibir las nuevas curvas con nombre.

    ![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 uso de las preferencias de directiva de grupo para distribuir curvas*

## <a name="managing-tls-ecc-order"></a>Administrar el orden ECC de TLS

A partir de Windows 10 y Windows Server 2016, se puede usar la configuración de la Directiva de grupo de orden de curva ECC para configurar el orden de curva ECC TLS predeterminado. Mediante el uso de ECC genérico y este valor, las organizaciones pueden agregar sus propias curvas con nombre de confianza (que están aprobadas para su uso con TLS) al sistema operativo y, a continuación, agregar esas curvas con nombre al valor de prioridad de curva directiva de grupo para asegurarse de que se usan en un TLS posterior. protocolos. Las nuevas listas de prioridades de curva se activan en el siguiente reinicio después de recibir la configuración de la Directiva.     

![GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 administración de la prioridad de curva TLS mediante directiva de grupo*


