---
title: Instalar certificados de raíz de confianza de TPM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/21/2019
ms.openlocfilehash: 211d2f24b01d1e308f012df681f9e16a2190449f
ms.sourcegitcommit: 260b1d78cb28b88b876579e1ac9a41a74e8752fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2019
ms.locfileid: "67398806"
---
# <a name="install-trusted-tpm-root-certificates"></a>Instalar certificados de raíz de confianza de TPM

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al configurar HGS para usar la atestación de TPM, también deberá configurar HGS para confiar en los proveedores de los TPM en sus servidores.
Este proceso de comprobación adicional garantiza que sólo los TPM auténticos y confiables puedan dar fe en el HGS.
Si se intenta registrar un TPM de confianza con `Add-HgsAttestationTpmHost`, recibirá un error que indica el fabricante TPM no es de confianza.

Para confiar en los TPM, la raíz y certificados de firma intermedios utilizados para firmar la clave de aprobación de TPM de los servidores deben instalarse en HGS.
Si usa más de un modelo TPM en su centro de datos, deberá instalar certificados diferentes para cada modelo.
HGS buscará en la sección "TrustedTPM_RootCA" y "TrustedTPM_IntermediateCA" certificado se almacena para los certificados del proveedor.

> [!NOTE]
> Los certificados de proveedor TPM son diferentes de los que se instala de forma predeterminada en Windows y representan la raíz específica y los certificados intermedios utilizados por los proveedores TPM.

Una colección de certificados intermedios y raíz de confianza TPM publicada por Microsoft para su comodidad.
Puede usar los pasos siguientes para instalar estos certificados.
Si los certificados TPM no se incluyen en el paquete, a continuación, póngase en contacto con el proveedor TPM o el servidor OEM para obtener la raíz y los certificados intermedios para su modelo específico de TPM.

Repita los pasos siguientes en **cada servidor HGS**:

1.  Descargue el paquete más reciente de [ https://tpmsec.microsoft.com/TPMCerts/TrustedTPM.cab ](https://tpmsec.microsoft.com/TPMCerts/TrustedTPM.cab).

2.  Expanda el archivo cab.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  De forma predeterminada, el script de configuración instalará los certificados para cada proveedor TPM. Si solo desea importar los certificados de su proveedor específico de TPM, elimine las carpetas para los proveedores TPM que no confía en su organización.

4.  Instale el paquete de certificado de confianza mediante la ejecución del script de instalación en la carpeta expandida.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Para agregar nuevos certificados o las intencionadamente omitido durante una instalación anterior, solo tiene que repetir los pasos anteriores en todos los nodos del clúster de HGS.
Los certificados existentes permanecerán confianza, pero se agregarán nuevos certificados que se encuentra en el archivo .cab expandido en el TPM de confianza almacena.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar el tejido DNS](guarded-fabric-configuring-fabric-dns-tpm.md)



