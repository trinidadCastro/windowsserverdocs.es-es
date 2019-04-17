---
title: Administrar los certificados de Software definido redes
description: Puedes usar este tema para aprender a administrar certificados Northbound de controlador de red y comunicaciones Southbound al implementar Software definido de redes (SDN) en Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3036de9161cabc2f3a85a1d3b2ce7739f0ff6bd3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-for-software-defined-networking"></a>Administrar los certificados de Software definido redes

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo administrar certificados Northbound de controlador de red y comunicaciones Southbound cuando implementas \(SDN\) Software definido de redes en Windows Server 2016 Datacenter y estás usando System Center Virtual Machine Manager \(SCVMM\) como cliente de administración SDN.

>[!NOTE]
>Para obtener información general sobre controladores de red, consulta [controlador de red](../technologies/network-controller/Network-Controller.md).

Si no estás usando Kerberos para proteger la comunicación entre el controlador de red, puedes usar certificados X.509 para la autenticación, autorización y cifrado.

SDN en Windows Server 2016 Datacenter admite ambas versiones firmado self\ y entidad de certificación \ (CA\)-certificados X.509 firmados. Este tema proporciona instrucciones paso a paso para crear estos certificados y aplicarlos para proteger los canales de comunicación de red controlador Northbound con clientes de administración y comunicaciones Southbound con dispositivos de red, como la \(SLB\) equilibrado de carga de Software.
.
Cuando se usa la autenticación basada en certificate\, debe inscribir un certificado en nodos de controlador de red que se usa en las siguientes maneras.

1. Cifrar la comunicación Northbound con capa de Sockets seguros \(SSL\) entre los nodos de controlador de red y los clientes de administración, como System Center Virtual Machine Manager.
2. Autenticación entre los nodos de controlador de red y Southbound dispositivos y servicios, como hosts de Hyper-V y \(SLBs\) equilibradores de carga de Software.

## <a name="creating-and-enrolling-an-x509-certificate"></a>Crear e inscribir un certificado X.509

Puedes crear e inscribir un certificado de firma de self\ o un certificado emitido por una entidad de certificación.

>[!NOTE]
>Cuando usas SCVMM para implementar el controlador de red, debes especificar el certificado X.509 que se usa para cifrar las comunicaciones Northbound durante la configuración de la plantilla de servicio del controlador de red.

La configuración de certificado debe incluir los siguientes valores.

- El valor de la **RestEndPoint** cuadro de texto debe ser el \(FQDN\) red controlador nombre de dominio completo o dirección IP. 
- La **RestEndPoint** valor debe coincidir con el nombre del sujeto \ (nombre común, CN\) del certificado X.509.

### <a name="creating-a-self-signed-x509-certificate"></a>Cómo crear un certificado de firma de Self\ X.509

Puedes crear un certificado X.509 autofirmado y exportarla con la clave privada \ (protegido con una password\) siguiendo estos pasos para implementaciones single\ y multiple\ nodos de controlador de red.

Al crear certificados firmados self\, puedes usar las siguientes directrices.

- Puedes usar la dirección IP del extremo del resto de controlador de red para el parámetro DnsName - pero no se recomienda porque requiere que los nodos de controlador de red son dentro de una subred solo administración \ (por ejemplo, en una sola rack\)
- Para varias implementaciones de nodo CN, el nombre DNS que especifiques se convertirá en el nombre completo del clúster de controlador de red \ (registros DNS Host se crean automáticamente. \) 
- Para las implementaciones de controlador de red de nodo único, el nombre DNS puede ser el nombre de host del controlador de red seguido del nombre de dominio completo.

#### <a name="multiple-node"></a>Varios nodos

Puedes usar la [nueva SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando de Windows PowerShell para crear un certificado de firma de self\.

**Sintaxis**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**Ejemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>Nodo único

Puedes usar la [nueva SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) comando de Windows PowerShell para crear un certificado de firma de self\.

**Sintaxis**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**Ejemplo de uso**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>Cómo crear un certificado de firma de CA\ X.509

Para crear un certificado con una entidad de certificación, debe haber implementado un \(PKI\) infraestructura de clave pública con los servicios de certificados de Active Directory \(AD CS\). 

>[!NOTE]
>Entidades emisoras de certificados de terceros o herramientas, como openssl, puedes usar para crear un certificado para su uso con el controlador de red, sin embargo, las instrucciones de este tema son específicas de AD CS. Para obtener información sobre cómo usar una CA de terceros o la herramienta, consulta la documentación para el software que usas.

Crear un certificado con una entidad de certificación incluye los siguientes pasos.

1. Puedes o de su organización dominio o el Administrador de seguridad configura la plantilla de certificado
2. Puedes o administrador de controlador de red o de tu organización SCVMM administrador solicita un nuevo certificado de la CA.

#### <a name="certificate-configuration-requirements"></a>Requisitos de configuración de certificado

Mientras se configura una plantilla de certificado en el siguiente paso, asegúrate de que la plantilla que configuras incluye los siguientes elementos necesarios.

1. El nombre de firmante del certificado debe ser el FQDN del host Hyper-V
2. El certificado debe colocarse en el almacén personal del equipo local (mi – certificado: \localmachine\my)
3. El certificado debe tener ambos autenticación de servidor (EKU: 1.3.6.1.5.5.7.3.1) y la autenticación de cliente (EKU: 1.3.6.1.5.5.7.3.2) las directivas de aplicación.

>[!NOTE]
>Si el Personal \ (mi – certificado: \localmachine\my\) certificado de la tienda en el host Hyper\-V tiene X.509 más de un certificado con el nombre de asunto (CN) que el host del nombre de dominio completo \(FQDN\), asegúrate de que el certificado que usará SDN tiene una propiedad personalizada adicional de uso mejorado de claves con el OID 1.3.6.1.4.1.311.95.1.1.1. De lo contrario, la comunicación entre el controlador de red y el host no funcionen.

#### <a name="to-configure-the-certificate-template"></a>Para configurar la plantilla de certificado
  
>[!NOTE]
>Antes de realizar este procedimiento, debes revisar los requisitos de certificados y las plantillas de certificado disponible en la consola de plantillas de certificado. Puede modificar una plantilla existente o crear un duplicado de una plantilla existente y, a continuación, modificar tu copia de la plantilla. Se recomienda crear una copia de una plantilla existente.

1. En el servidor donde está instalado AD CS, en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **entidad de certificación**. Abre el \(MMC\) Microsoft Management Console de entidad emisora. 
2. En la consola de MMC, haga doble clic en el nombre de entidad emisora de certificados, haz clic en **plantillas de certificado**y, a continuación, haz clic en **administrar**.
3. Abre la consola de plantillas de certificado. Todas las plantillas de certificado se muestran en el panel de detalles.
4. En el panel de detalles, haz clic en la plantilla que quieras duplicar.
5.  Haz clic en el **acción** menú y, a continuación, haz clic en **plantilla duplicada**. La plantilla **propiedades** abre el cuadro de diálogo.
6.  En la plantilla **propiedades** cuadro de diálogo, en la **nombre de sujeto** , haga clic **proporcionado por el solicitante**. \ (Esta configuración es necesaria para certificados SSL del controlador de red. \)
7.  En la plantilla **propiedades** cuadro de diálogo, en la **tratamiento de la solicitud** pestaña, asegúrate de que **permitir que la clave privada se pueda exportar** está seleccionado. Asegúrate también de que la **firma y cifrado** propósito se selecciona.
8.  En la plantilla **propiedades** cuadro de diálogo, en la **extensiones** , selecciona **uso de clave**y, a continuación, haz clic en **editar**.
9.  En **firma**, asegúrate de que **firma Digital** está seleccionado.
10.  En la plantilla **propiedades** cuadro de diálogo, en la **extensiones** , selecciona **las directivas de aplicación**y, a continuación, haz clic en **editar**.
11.  En **las directivas de aplicación**, asegúrate de que **autenticación de cliente** y **autenticación de servidores** aparecen.
12.  Guardar una copia de la plantilla de certificado con un nombre único, como **plantilla de controlador de red**.

#### <a name="to-request-a-certificate-from-the-ca"></a>Para solicitar un certificado de la entidad de certificación

Puedes usar el complemento certificados para solicitar certificados. Puedes solicitar cualquier tipo de certificado que se ha configurado previamente y se ponen a disposición un administrador de la CA que procesa la solicitud de certificado.

**Los usuarios** o local **administradores** es la pertenencia al grupo mínima necesaria para completar este procedimiento.

1. Abra el complemento certificados para un equipo.
2. En el árbol de consola, haz clic en **certificados \(Local Computer\)**. Selecciona el **Personal** almacén de certificados.
3. En la **acción** menú, elija ** todas las tareas ** y, a continuación, haz clic en **solicitar un nuevo certificado** para iniciar el Asistente de inscripción de certificados. Haz clic en **siguiente**.
4. Selecciona el **configurado por el administrador** directiva de inscripción de certificado y haz clic en **siguiente**.
5. Selecciona el **directiva de inscripción de Active Directory** \ (basado en la plantilla de entidad emisora de certificados que has configurado en el section\ anterior).
6. Expande la **detalles** sección y configurar los siguientes elementos.
    1. Asegúrate de que **uso de claves** incluye ** firma Digital ** y **cifrado de clave**.
    2. Asegúrate de que **las directivas de aplicación** incluye **autenticación de servidores** \(1.3.6.1.5.5.7.3.1\) y **autenticación de cliente** \(1.3.6.1.5.5.7.3.2\).
7. Haz clic en **propiedades**.
8. En la **asunto** pestaña **nombre de sujeto**, en **tipo**, selecciona **nombre común**. En el valor, especifica **extremo REST de controlador de la red**.
9. Haz clic en **aplicar**y, a continuación, haz clic en **Aceptar**.
10. Haz clic en **inscribir**.

En la consola de MMC de certificados, haz clic en el almacén Personal para ver el certificado que te has inscrito a la CA.

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>Exportar y copia el certificado a la biblioteca SCVMM

Después de crear un certificado de firma self\ o firmado CA\, debe exportar el certificado con la clave privada \(in.pfx format\) y sin la clave privada \ (en Base64 .cer format\) desde el complemento de certificados. 

A continuación, debes copiar los dos archivos exportados a la **ServerCertificate.cr** y **NCCertificate.cr** carpetas que especificaste en el momento cuando se importa la plantilla de servicio NC.

1. Abre el complemento de certificados (certlm.msc) y busque el certificado en el almacén de certificados Personal para el equipo local.
2. Right\ y haga clic en el certificado, haz clic en **todas las tareas**y, a continuación, haz clic en **exportar**. Abre el Asistente de exportación de certificado. Haz clic en **siguiente**.
3. Selecciona **Sí**, exportar la opción de clave privada, haz clic en **siguiente**.
4. Elige **intercambio de información Personal: PKCS #12 (. PFX)** y aceptar el valor predeterminado de **incluir todos los certificados en la ruta de acceso de certificación** si es posible.
5. Asignar los grupos de usuarios y una contraseña para el certificado que se va a exportar, haz clic en **siguiente**.
6. En el archivo para exportar la página, busca la ubicación donde quieras colocar el archivo exportado y asignarle un nombre.
7. Del mismo modo, exportar el certificado. Formato CER. Nota: Para exportar a. Formato CER, desactiva la Sí, exportar la opción de clave privada.
8. Copia el. PFX a la carpeta ServerCertificate.cr.
9. Copia el. Archivo CER a la carpeta NCCertificate.cr.

Cuando hayas terminado, actualizar estas carpetas en la biblioteca de SCVMM y asegúrate de que estos certificados copiados. Continuar con la configuración de plantilla del servicio de controlador de red y la implementación.

## <a name="authenticating-southbound-devices-and-services"></a>Autenticar servicios y dispositivos Southbound 

Comunicación de controlador de red con hosts y dispositivos SLB MUX usa certificados para la autenticación. Comunicación con los hosts es sobre el protocolo OVSDB mientras la comunicación con los dispositivos SLB MUX es a través del protocolo WCF.

### <a name="hyper-v-host-communication-with-network-controller"></a>Comunicación con el Host de Hyper-V con el controlador de red

Para la comunicación con los hosts de Hyper-V sobre OVSDB, controlador de red debe presentar un certificado en los equipos host. De manera predeterminada, SCVMM retoma el certificado SSL configurado en el controlador de red y lo usa para la comunicación southbound con los hosts.

Esa es la razón por qué el certificado SSL debe tener el EKU de autenticación de cliente configurado. Este certificado está configurado en el resto de "servidores" recursos \ (hosts de Hyper-V se representan en el controlador de red como un resource\ de servidor) y pueden verse ejecutando el comando de Windows PowerShell **Get NetworkControllerServer**.

Siguiente es un ejemplo parcial del servidor recursos REST.

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Para la autenticación mutua, el host de Hyper-V también debe tener un certificado para comunicarse con el controlador de red. 

Puedes inscribir el certificado de una entidad de certificación de \(CA\). Si no se encuentra un certificado de entidad emisora de certificados basado en el equipo host, SCVMM crea un certificado autofirmado y aprovisiona en el equipo host.

Controlador de red y los certificados de host de Hyper-V deben ser de confianza entre sí. Certificado de raíz del certificado de host de Hyper-V debe estar presente en las entidades emisoras de raíz de confianza de controlador de red de la tienda para el equipo Local y viceversa. 

Cuando usas certificados firmados self\, SCVMM garantiza que los certificados necesarios están presentes en el almacén de entidades de certificación raíz de confianza para el equipo Local. 

Si estás usando los certificados de CA en función de los hosts de Hyper-V, debes asegurarte de que el certificado de entidad emisora de certificados raíz está presente en el almacén de entidades de certificación raíz de confianza del controlador de red para el equipo Local.

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>Software carga equilibrado MUX comunicación con el controlador de red

El Software carga equilibrado Multiplexor \(MUX\) y el controlador de red comunican a través del protocolo WCF, uso de certificados para la autenticación.

De manera predeterminada, SCVMM retoma el certificado SSL configurado en el controlador de red y lo usa para la comunicación southbound con los dispositivos de multiplexación. Este certificado está configurado en el "NetworkControllerLoadBalancerMux" recursos de REST y puede verse al ejecutar el cmdlet de Powershell **Get NetworkControllerLoadBalancerMux**.

Ejemplo de REST MUX recursos \(partial\):

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

Para la autenticación mutua, también debe tener un certificado en los dispositivos SLB MUX. Este certificado se configura automáticamente SCVMM cuando implementas equilibrado de carga de software con SCVMM.

>[!IMPORTANT]
>En el host y nodos SLB, es fundamental que el almacén de certificados de entidades de certificación raíz de confianza no incluye ningún certificado donde "Emitido para" no es el mismo que "Emitido por". Si esto ocurre, se produce un error en la comunicación entre el controlador de red y el dispositivo southbound.

Controlador de red y los certificados SLB MUX deben ser de confianza entre sí \ (certificado de raíz del certificado SLB MUX debe estar presente en la máquina de controlador de red de entidades de certificación raíz de confianza de la tienda y versa\). Cuando usas certificados firmados self\, SCVMM garantiza que los certificados necesarios están presentes en el de las entidades de certificación raíz de confianza de la tienda para el equipo Local.



