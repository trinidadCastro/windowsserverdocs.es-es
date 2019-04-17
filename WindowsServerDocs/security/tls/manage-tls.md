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
ms.date: 06/05/2017
ms.openlocfilehash: e26d1f833dbb7219251947f1cb3cb09def958aea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-transport-layer-security-tls"></a>Administrar la seguridad de la capa de transporte (TLS)

## <a name="configuring-tls-cipher-suite-order"></a>Configurar orden de conjuntos de cifrado TLS

Diferentes versiones de Windows admiten distintos conjuntos de cifrado TLS y orden de prioridad. Consulta [conjuntos de cifrado de TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx) para el orden predeterminado admitido por Microsoft Schannel Provider en diferentes versiones de Windows.

> [!NOTE] 
> También puedes modificar la lista de conjuntos de cifrado mediante el uso de funciones CNG, consulta [dar prioridad a conjuntos de cifrado Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx) para obtener más información.

Los cambios en el orden de conjunto de cifrado TLS surtirán efecto en el próximo arranque. Hasta el reinicio o apagado, el orden existente estará en vigor.

> [!WARNING] 
> Actualizar la configuración del registro para el orden de prioridad predeterminado, no se admite y se puede restablecer con las actualizaciones de mantenimiento. 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>Configurar el orden de conjuntos de cifrado de TLS mediante la directiva de grupo

Puedes usar la configuración de directiva de grupo de orden de SSL cifrado conjunto para configurar el orden de conjunto de cifrado TLS predeterminado.

1.  Desde la consola de administración de directivas de grupo, ve a **configuración del equipo** > **plantillas administrativas** > **redes** > **opciones de configuración de SSL**.
2.  Haz doble clic en **orden de conjuntos de cifrado SSL**y, a continuación, haz clic en el **habilitado** opción.
3.  Haz clic en **conjuntos de cifrado SSL** cuadro y selecciona **seleccionar todo** en el menú emergente.

    ![Configuración de directiva de grupo](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  Haz clic en el texto seleccionado y selecciona **copia** en el menú emergente.
5.  Pegar el texto en un editor de texto como notepad.exe y actualización con la nueva lista de orden de conjunto de cifrado.

    > [!NOTE]
    > La lista de orden de conjunto de cifrado TLS debe estar en formato delimitado por estricta de comas. Cada cadena de conjunto de cifrado finalizará con una coma (,) en el lado derecho de la misma. 

    > Además, la lista de los conjuntos de cifrado se limita a 1.023 caracteres.

6.  Reemplazar la lista en la **conjuntos de cifrado SSL** con la lista ordenada actualizada.
7.  Haz clic en **Aceptar** o **aplicar**.

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>Configuración de orden de conjuntos de cifrado de TLS con MDM

El CSP de directivas de Windows 10 admite la configuración de los conjuntos de cifrado de TLS. Consulta [criptografía/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites) para obtener más información.

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>Configurar el orden de conjuntos de cifrado de TLS mediante los PowerShell Cmdlets de TLS

El módulo de PowerShell de TLS admite obtener la lista ordenada de conjuntos de cifrado TLS, deshabilitar un conjunto de cifrado y habilitar un conjunto de cifrado. Consulta [TLS módulo](https://technet.microsoft.com/itpro/powershell/windows/tls/tls) para obtener más información.

## <a name="configuring-tls-ecc-curve-order"></a>Configurar el orden de la curva de TLS ECC 

A partir de Windows 10 y Windows Server 2016, ECC curva orden se puede configurar independiente del orden de conjunto de cifrado. Si la lista tiene sufijos de curva elíptica el orden de conjuntos de cifrado de TLS, se reemplazará por el nuevo orden de prioridad de curva elíptica, cuando se habilita. Esto permite a las organizaciones usa un objeto de directiva de grupo para configurar diferentes versiones de Windows con el mismo orden de los conjuntos de cifrado.

> [!NOTE]
> Antes de Windows 10, se anexan cadenas de conjunto de cifrado con la curva elíptica para determinar la prioridad de la curva.

### <a name="managing-windows-ecc-curves-using-certutil"></a>Administración de curvas Windows ECC con CertUtil

A partir de Windows 10 y Windows Server 2016, Windows proporciona administración de parámetro de curva elíptica aunque la certuil.exe Utilidad línea de comandos. Parámetros de curva elíptica se almacenan en la bcryptprimitives.dll. Utilizando certutil.exe, los administradores pueden agregar y quitar parámetros de la curva hacia y desde Windows, respectivamente. Certutil.exe almacena los parámetros de la curva de forma segura en el registro. Windows puede empezar a usar los parámetros de la curva por el nombre asociado a la curva.    

#### <a name="displaying-registered-curves"></a>Mostrar curvas registrados

Usa el siguiente comando certutil.exe para mostrar una lista de las curvas registrada para el equipo actual.

```powershell
certutil.exe –displayEccCurve
```

![Certutil pantalla curvas](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*Figura 1 Certutil.exe de salida para mostrar la lista de las curvas registradas.*

#### <a name="adding-a-new-curve"></a>Agregar una nueva curva

Las organizaciones pueden crear y usar parámetros de la curva investigados otras entidades de confianza.  
Los administradores que deseen usar estas nuevas curvas en Windows deben agregar la curva.  
Para agregar una curva equipo actual, usa el siguiente comando certutil.exe:

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- La **curveName** argumento representa el nombre de la curva en la que se agregaron los parámetros de la curva.
- La **curveParameters** argumento representa el nombre de archivo de un certificado que contiene los parámetros de las curvas que deseas agregar.
- La **curveOid** argumento representa un nombre de archivo de un certificado que contiene el OID de los parámetros de la curva que desea agregar (opcional).
- La **curveType** argumento representa un valor decimal de la curva con nombre de la [registro de curva denominado CE](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8) (opcional).

![Certutil agregar curvas](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*Figura 2 de agregar una curva con certutil.exe.*

#### <a name="removing-a-previously-added-curve"></a>Quitar una curva previamente agregada

Los administradores pueden quitar una curva previamente agregada mediante el siguiente comando certutil.exe:

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows no puede usar una curva con nombre después de que un administrador quita la curva de equipo.

## <a name="managing-windows-ecc-curves-using-group-policy"></a>Administración de curvas ECC de Windows mediante la directiva de grupo

Las organizaciones pueden distribuir los parámetros de la curva a enterprise, Unidos al dominio mediante la directiva de grupo y la extensión del registro de preferencias de directiva de grupo del equipo.  
El proceso para distribuir una curva es:

1.  En Windows 10 y Windows Server 2016, usa **certutil.exe** para agregar una nueva curva registrada con nombre en Windows.
2.  Desde ese mismo equipo, abre la consola de administración de directivas de grupo (GPMC), crea un nuevo objeto de directiva de grupo y editarlo.
3.  Navegar a **configuración del equipo | Preferencias | Configuración de Windows | Registro**.  Haz clic en **registro**. Mantener el mouse sobre **nueva** y selecciona **artículo de colección**. El nombre del elemento de colección para que coincida con el nombre de la curva. Se crea un elemento de colección del registro para cada clave del registro bajo *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*.
4.  Configurar la colección de registro de preferencia de directiva de grupo recién creado agregando un nuevo **elemento del registro** de cada valor del registro enumerados en *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\ [curveName]*.
5.  Implementar el objeto de directiva de grupo que contiene el elemento de colección de registro de directiva de grupo para equipos de Windows 10 y Windows Server 2016 que deberían recibir las curvas con nombre nuevo.

    ![3GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *Figura 3 preferencias de directiva de grupo usando distribuir curvas*

## <a name="managing-tls-ecc-order"></a>Administrar el orden de TLS ECC

A partir de Windows 10 y Windows Server 2016, la directiva de grupo de orden de la curva de ECC configuración puede usarse configurar TLS ECC curva orden predeterminado. Usando ECC genérico y esta configuración, las organizaciones, se puede agregar sus propios confianza denominado curvas (que están aprobadas para su uso con TLS) para el sistema operativo y agregarlas denominado curvas para la configuración de directiva de grupo de curva prioridad para garantizar que se usen protocolos de enlace TLS futuras. En el siguiente reinicio después de recibir la configuración de directiva se activarán nuevas listas de prioridad de la curva.     

![3GPP distribuir curvas](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*Figura 4 administrar TLS una curva prioridad mediante Directiva de grupo*


