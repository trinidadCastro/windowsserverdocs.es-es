---
title: Instalar certificados raíz de TPM de confianza
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 15614ce1065170bc557fad10a168b3dda6a5b05a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386551"
---
# <a name="install-trusted-tpm-root-certificates"></a>Instalar certificados raíz de TPM de confianza

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al configurar HGS para usar la atestación de TPM, también debe configurar HGS para confiar en los proveedores de los TPM de los servidores.
Este proceso de comprobación adicional garantiza que solo los TPM auténticos y confiables pueden atestiguarse con su HGS.
Si intenta registrar un TPM que no es de confianza con `Add-HgsAttestationTpmHost`, recibirá un error que indica que el proveedor de TPM no es de confianza.

Para confiar en los TPM, los certificados de firma intermedios y raíz utilizados para firmar la clave de aprobación en los TPM de los servidores deben instalarse en HGS.
Si usa más de un modelo de TPM en su centro de información, puede que necesite instalar certificados diferentes para cada modelo.
HGS buscará los certificados de proveedor en los almacenes de certificados "TrustedTPM_RootCA" y "TrustedTPM_IntermediateCA".

> [!NOTE]
> Los certificados de proveedor de TPM son diferentes de los que se instalan de forma predeterminada en Windows y representan los certificados raíz e intermedios específicos utilizados por los proveedores de TPM.

Microsoft publica una colección de certificados raíz e intermedios de TPM de confianza para su comodidad.
Puede usar los pasos siguientes para instalar estos certificados.
Si los certificados de TPM no se incluyen en el paquete siguiente, póngase en contacto con el proveedor de TPM o el OEM del servidor para obtener los certificados raíz e intermedio para el modelo de TPM específico.

Repita los pasos siguientes en **cada servidor HGS**:

1.  Descargue el paquete más reciente de [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

2.  Compruebe la firma del archivo. cab para garantizar su autenticidad. No continúe si la firma no es válida.

    ```powershell
    Get-AuthenticodeSignature .\TrustedTpm.cab
    ```
    
    Este es un ejemplo de los resultados:
    
    ```
    Directory: C:\Users\Administrator\Downloads
        
    SignerCertificate                         Status                                 Path
    -----------------                         ------                                 ----
    0DD6D4D4F46C0C7C2671962C4D361D607E370940  Valid                                  TrustedTpm.cab
    ```

2.  Expanda el archivo CAB.

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  De forma predeterminada, el script de configuración instalará certificados para todos los proveedores de TPM. Si solo desea importar certificados para su proveedor de TPM específico, elimine las carpetas de los proveedores de TPM que no sean de confianza para su organización.

4.  Instale el paquete de certificado de confianza mediante la ejecución del script de instalación en la carpeta expandida.

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

Para agregar nuevos certificados o que se omitieron intencionadamente durante una instalación anterior, simplemente Repita los pasos anteriores en cada nodo del clúster de HGS.
Los certificados existentes seguirán siendo de confianza, pero los nuevos certificados encontrados en el archivo. cab expandido se agregarán a los almacenes de TPM de confianza.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Configurar el tejido DNS](guarded-fabric-configuring-fabric-dns-tpm.md)



