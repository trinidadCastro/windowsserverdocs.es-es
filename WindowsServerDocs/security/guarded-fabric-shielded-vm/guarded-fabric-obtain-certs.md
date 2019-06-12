---
title: Obtener certificados de HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2dc232eb7aeb8b0807a8e9989ae3dc893f925f66
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447365"
---
# <a name="obtain-certificates-for-hgs"></a>Obtener certificados de HGS

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Al implementar HGS, se le pedirá que proporcione certificados de firma y cifrado que se usan para proteger la información confidencial necesaria para iniciar una máquina virtual blindada.
Estos certificados nunca deje HGS y solo se usan para descifrar blindada claves de máquina virtual cuando el host en el que se ejecutan ha demostrado que su estado es correcto.
Los inquilinos (propietarios de la máquina virtual) usar el público en la mitad de los certificados para autorizar a su centro de datos para ejecutar sus máquinas virtuales blindadas.
En esta sección se trata los pasos necesarios para obtener certificados de firma y cifrado compatibles de HGS.

## <a name="request-certificates-from-your-certificate-authority"></a>Solicitar certificados de la entidad emisora de certificados

Aunque no es necesario, se recomienda encarecidamente que obtener los certificados de una entidad de certificación de confianza.
Esto ayuda a los propietarios de la máquina virtual a comprobar que está autorizando el servidor HGS correcto (es decir, el proveedor de servicios o el centro de datos) para ejecutar sus máquinas virtuales blindadas.
En un escenario empresarial, puede utilizar su propia CA de empresa para emitir estos certificados.
Los proveedores de hospedaje y proveedores de servicios deben considerar el uso de una entidad de certificación pública, conocido en su lugar.

(A menos que el marcado como "recomendada") deben emitir certificados de la firma y el cifrado con las siguientes propiedades de certificado:

Propiedad de la plantilla de certificado | Valor requerido 
------------------------------|----------------
Proveedor de cifrado               | Cualquier proveedor de almacenamiento de claves (KSP). Proveedores de servicios criptográficos (CSP) heredados son **no** compatibles.
Algoritmo de clave                 | RSA
Tamaño mínimo de clave              | 2048 bits
Algoritmo de firma           | Recomendado: SHA256
Uso de la clave                     | Firma digital *y* cifrado de datos
Uso mejorado de clave            | Autenticación de servidor
Directiva de renovación de clave            | Renovar con la misma clave. Renovación de certificados HGS con claves diferentes impedirá que las máquinas virtuales blindadas iniciarse.
Nombre de sujeto                  | Recomendado: dirección de su compañía nombre o web. Esta información se mostrará a los propietarios de la máquina virtual en el Asistente para archivos de datos blindaje.

Estos requisitos se aplican si está utilizando certificados respaldados por hardware o software.
Por motivos de seguridad, se recomienda crear las claves HGS en un módulo de seguridad de Hardware (HSM) para impedir que el sistema que se va a copiar las claves privadas.
Siga las instrucciones de su proveedor HSM para solicitar certificados con los atributos anteriores y asegúrese de instalar y autorizar a los HSM KSP en cada nodo HGS.

Cada nodo HGS requerirá acceso a la misma firma y certificados de cifrado.
Si utiliza certificados de software de seguridad, puede exportar los certificados a un archivo PFX con una contraseña y permitir HGS administrar los certificados automáticamente.
También puede instalar los certificados en el almacén de certificados del equipo local en cada nodo HGS y proporcionar la huella digital para HGS.
Ambas opciones se explican en la [inicializar el clúster de HGS](guarded-fabric-initialize-hgs.md) tema.

## <a name="create-self-signed-certificates-for-test-scenarios"></a>Crear certificados autofirmados para escenarios de prueba

Si está creando un entorno de laboratorio HGS y no tiene o va a usar una entidad de certificación, puede crear certificados autofirmados.
Recibirá una advertencia al importar la información del certificado en el Asistente para archivos de datos blindaje, pero toda la funcionalidad seguirán siendo las mismas.

Para crear certificados autofirmados y exportarlos a un archivo PFX, ejecute los siguientes comandos de PowerShell:

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

Todas las claves y la información confidencial que se transmiten entre hosts de Hyper-V y HGS se cifran en el nivel de mensaje; es decir, la información se cifra con claves que se conoce a HGS o Hyper-V, evitar que alguien esté examinando su tráfico de red y el robo de claves para las máquinas virtuales.
Sin embargo, si tiene compatibilidad reqiurements o simplemente prefiere cifrar todas las comunicaciones entre Hyper-V y HGS, puede configurar HGS con un certificado SSL que se cifrará todos los datos en el nivel de transporte.

Tanto los hosts de Hyper-V y nodos HGS deberá confiar en el certificado SSL que se proporciona, por lo que se recomienda que solicite el certificado SSL de la entidad de certificación empresarial. Cuando se solicita el certificado, asegúrese de especificar lo siguiente:

Propiedad de certificado SSL | Valor requerido
-------------------------|---------------
Nombre de sujeto             | Nombre del clúster HGS (nombre de red distribuida). Esto será una concatenación del nombre de servicio de HGS proporcionado a `Initialize-HgsServer` y el nombre de dominio HGS.
Nombre alternativo del sujeto | Si va a utilizar un nombre DNS diferente para llegar a su clúster HGS (por ejemplo, si está detrás de un equilibrador de carga), no olvide incluir los nombres DNS en el campo de SAN de la solicitud de certificado.

En el que se tratan las opciones para especificar este certificado al inicializar el servidor HGS [configurar el primer nodo HGS](guarded-fabric-initialize-hgs.md).
También puede agregar o cambiar el certificado SSL en un momento posterior mediante el [conjunto HgsServer](https://docs.microsoft.com/powershell/module/hgsserver/set-hgsserver?view=win10-ps) cmdlet.

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Instalar HGS](guarded-fabric-choose-where-to-install-hgs.md)