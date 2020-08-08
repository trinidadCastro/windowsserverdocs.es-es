---
title: Obtener certificados para HGS
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 09/25/2019
ms.openlocfilehash: 995a115b9e611500732e4674880ee4ca4b204e14
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989114"
---
# <a name="obtain-certificates-for-hgs"></a>Obtener certificados para HGS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al implementar HGS, se le pedirá que proporcione certificados de firma y cifrado que se usan para proteger la información confidencial necesaria para iniciar una máquina virtual blindada.
Estos certificados nunca salen del HGS y solo se usan para descifrar las claves de las máquinas virtuales blindadas cuando el host en el que se ejecutan ha demostrado que está en buen estado.
Los inquilinos (propietarios de VM) usan la mitad pública de los certificados para autorizar a su centro de recursos a ejecutar sus máquinas virtuales blindadas.
En esta sección se describen los pasos necesarios para obtener certificados de cifrado y firma compatibles para HGS.

## <a name="request-certificates-from-your-certificate-authority"></a>Solicitar certificados de la entidad de certificación

Aunque no es necesario, se recomienda encarecidamente obtener los certificados de una entidad de certificación de confianza.
Esto ayuda a los propietarios de máquinas virtuales a comprobar que están autorizando el servidor de HGS correcto (es decir, un proveedor de servicios o un centro de recursos) para ejecutar sus máquinas virtuales blindadas.
En un escenario empresarial, puede optar por usar su propia entidad de certificación empresarial para emitir estos certificados.
Los proveedores de servicios de hosting y de servicio deben considerar el uso de una entidad de certificación pública conocida en su lugar.

Se deben emitir los certificados de firma y cifrado con las siguientes propiedades de certificiate (a menos que se marquen como "recomendadas"):

Propiedad de plantilla de certificado | Valor requerido
------------------------------|----------------
Proveedor de cifrado               | Cualquier proveedor de almacenamiento de claves (KSP). **No** se admiten proveedores de servicios de cifrado (CSP) heredados.
Algoritmo de clave                 | RSA
Tamaño mínimo de clave              | 2048 bits
Algoritmo de firma           | Recomendado: SHA256
Uso de las claves                     | Firma digital *y* cifrado de datos
Uso mejorado de clave            | Autenticación de servidor
Directiva de renovación de claves            | Renovar con la misma clave. La renovación de certificados HGS con claves diferentes impedirá que se inicien las máquinas virtuales blindadas.
Nombre de sujeto                  | Recomendado: el nombre o la dirección Web de su empresa. Esta información se mostrará a los propietarios de máquinas virtuales en el Asistente para archivos de datos de blindaje.

Estos requisitos se aplican tanto si usa certificados con el respaldo del hardware o software.
Por motivos de seguridad, se recomienda que cree las claves HGS en un módulo de seguridad de hardware (HSM) para evitar que se copien las claves privadas del sistema.
Siga las instrucciones del proveedor de HSM para solicitar certificados con los atributos anteriores y asegúrese de instalar y autorizar el KSP HSM en cada nodo de HGS.

Cada nodo de HGS necesitará acceso a los mismos certificados de firma y cifrado.
Si usa certificados respaldados por software, puede exportar los certificados a un archivo PFX con una contraseña y permitir que HGS administre los certificados automáticamente.
También puede optar por instalar los certificados en el almacén de certificados del equipo local en cada nodo de HGS y proporcionar la huella digital al HGS.
Ambas opciones se explican en el tema [Initialize The HGS Cluster](guarded-fabric-initialize-hgs.md) .

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Crear certificados autofirmados para escenarios de prueba

Si va a crear un entorno de laboratorio de HGS y no tiene o desea usar una entidad de certificación, puede crear certificados autofirmados.
Recibirá una advertencia al importar la información del certificado en el Asistente para archivos de datos de blindaje, pero todas las funciones seguirán siendo las mismas.

Para crear certificados autofirmados y exportarlos a un archivo PFX, ejecute los siguientes comandos en PowerShell:

```powershell
$certificatePassword = Read-Host -AsSecureString -Prompt "Enter a password for the PFX file"

$signCert = New-SelfSignedCertificate -Subject "CN=HGS Signing Certificate"
Export-PfxCertificate -FilePath .\signCert.pfx -Password $certificatePassword -Cert $signCert
Remove-Item $signCert.PSPath

$encCert = New-SelfSignedCertificate -Subject "CN=HGS Encryption Certificate"
Export-PfxCertificate -FilePath .\encCert.pfx -Password $certificatePassword -Cert $encCert
Remove-Item $encCert.PSPath
```

## <a name="request-an-ssl-certificate"></a>Solicitar un certificado SSL

Todas las claves y la información confidencial transmitida entre los hosts de Hyper-V y HGS se cifran en el nivel de mensaje; es decir, la información se cifra con claves conocidas para HGS o Hyper-V, lo que impide que alguien examine el tráfico de red y robe claves a las máquinas virtuales.
Sin embargo, si tiene reqiurements de cumplimiento o simplemente prefiere cifrar todas las comunicaciones entre Hyper-V y HGS, puede configurar HGS con un certificado SSL que cifrará todos los datos en el nivel de transporte.

Tanto los hosts de Hyper-V como los nodos HGS deben confiar en el certificado SSL que proporcione, por lo que se recomienda que solicite el certificado SSL de la entidad de certificación de la empresa. Al solicitar el certificado, asegúrese de especificar lo siguiente:

Propiedad de certificado SSL | Valor requerido
-------------------------|---------------
Nombre de sujeto             | Nombre del clúster de HGS (conocido como nombre de red distribuida o FQDN de objeto de equipo virtual). Esta será la concatenación del nombre del servicio HGS proporcionada a `Initialize-HgsServer` y el nombre de dominio de HGS.
Nombre alternativo del firmante | Si va a usar un nombre DNS diferente para llegar al clúster de HGS (por ejemplo, si se encuentra detrás de un equilibrador de carga), asegúrese de incluir esos nombres DNS en el campo SAN de la solicitud de certificado.

Las opciones para especificar este certificado al inicializar el servidor HGS se describen en [configuración del primer nodo HGS](guarded-fabric-initialize-hgs.md).
También puede Agregar o cambiar el certificado SSL en otro momento con el cmdlet [set-HgsServer](/powershell/module/hgsserver/set-hgsserver?view=win10-ps) .

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Instalar HGS](guarded-fabric-choose-where-to-install-hgs.md)