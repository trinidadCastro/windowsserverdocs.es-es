---
title: Configuración de Servicio web de inscripción de certificados para la renovación basada en claves de certificado en un puerto personalizado
description: ''
author: Deland-Han
ms.author: delhan
manager: dcscontentpm
ms.date: 11/12/2019
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: 3d3d08d6abe9daa571dd7365815c1fc61f926501
ms.sourcegitcommit: e5df3fd267352528eaab5546f817d64d648b297f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2019
ms.locfileid: "74163102"
---
# <a name="configuring-certificate-enrollment-web-service-for-certificate-key-based-renewal-on-a-custom-port"></a>Configuración de Servicio web de inscripción de certificados para la renovación basada en claves de certificado en un puerto personalizado

> Autores: Jitesh Thakur, Meera Mohideen, asesor técnico con el grupo de Windows.
Ingeniero de soporte técnico de Ankit Tyagi con el grupo de Windows

## <a name="summary"></a>Resumen

En este artículo se proporcionan instrucciones paso a paso para implementar el Servicio web de directiva de inscripción de certificados (de procesamiento de eventos complejos) y Servicio web de inscripción de certificados (CES) en un puerto personalizado distinto de 443 para la renovación basada en claves de certificado a fin de aprovechar las ventajas de la función automática característica de renovación de CEP y CES.

En este artículo también se explica cómo funciona el procesamiento de eventos complejos y CES y se proporcionan instrucciones de configuración.

> [!Note]
> El flujo de trabajo que se incluye en este artículo se aplica a un escenario específico. Es posible que el mismo flujo de trabajo no funcione para una situación diferente. Sin embargo, los principios siguen siendo los mismos.
>
> Declinación de responsabilidades: esta configuración se crea para un requisito específico en el que no desea usar el puerto 443 para la comunicación HTTPS predeterminada para los servidores de procesamiento de eventos complejos y CES. Aunque esta configuración es posible, tiene una compatibilidad limitada. Los servicios de atención al cliente y soporte técnico pueden ayudarle mejor si sigue esta guía con detenimiento la desviación mínima de la configuración de servidor Web proporcionada.

## <a name="scenario"></a>Escenario

En este ejemplo, las instrucciones se basan en un entorno que utiliza la configuración siguiente:

- Un bosque de Contoso.com que tenga una infraestructura de clave pública (PKI) de servicios de Certificate Server de Active Directory (AD CS).

- Dos instancias de CEP/CES configuradas en un servidor que se ejecuta en una cuenta de servicio. Una instancia utiliza el nombre de usuario y la contraseña para la inscripción inicial. El otro usa la autenticación basada en certificados para la renovación basada en claves en el modo de solo renovación.

- Un usuario tiene un grupo de trabajo o un equipo no unido a un dominio para el que va a inscribir el certificado de equipo mediante las credenciales de nombre de usuario y contraseña.

- La conexión del usuario a procesamiento de eventos complejos y CES a través de HTTPS se produce en un puerto personalizado, como 49999. (Este puerto se selecciona desde un intervalo de puertos dinámico y ningún otro servicio lo usa como puerto estático).

- Cuando la duración del certificado está llegando al final, el equipo usa la renovación basada en claves de CES basada en certificado para renovar el certificado a través del mismo canal.

![implantación](media/certificate-enrollment-certificate-key-based-renewal-1.png)

## <a name="configuration-instructions"></a>Instrucciones de configuración

### <a name="overview"></a>Introducción 

1. Configure la plantilla para la renovación basada en claves.

2. Como requisito previo, configure un servidor de procesamiento de eventos complejos y CES para la autenticación de nombre de usuario y contraseña.   
   En este entorno, se hace referencia a la instancia como "CEPCES01".

3.  Configure otra instancia de procesamiento de eventos complejos y CES mediante PowerShell para la autenticación basada en certificados en el mismo servidor. La instancia de CES usará una cuenta de servicio.

    En este entorno, se hace referencia a la instancia como "CEPCES02". La cuenta de servicio que se usa es "cepcessvc".

4.  Configure los valores del lado cliente.

### <a name="configuration"></a>Configuración

En esta sección se proporcionan los pasos para configurar la inscripción inicial.

> [!Note]
> También puede configurar cualquier cuenta de servicio de usuario, MSA o GMSA para que funcione CES.

Como requisito previo, debe configurar los CEP y CES en un servidor mediante la autenticación de nombre de usuario y contraseña.

#### <a name="configure-the-template-for-key-based-renewal"></a>Configuración de la plantilla para la renovación basada en claves

Puede duplicar una plantilla de equipo existente y configurar las siguientes opciones de la plantilla:

1. En la pestaña Nombre de sujeto de la plantilla de certificado, asegúrese de que esté seleccionada la opción **suministro en las opciones solicitar** y **usar información de asunto de los certificados existentes para las solicitudes de renovación de inscripción automática** .
   ![nuevas plantillas](media/certificate-enrollment-certificate-key-based-renewal-2.png) 

2. Cambie a la pestaña **requisitos de emisión** y, a continuación, active la casilla aprobación del administrador de certificados de entidad de **certificación** .
   ![requisitos de emisión](media/certificate-enrollment-certificate-key-based-renewal-3.png) 

3. Asigne el permiso de **lectura** e **inscripción** a la cuenta de servicio de **cepcessvc** para esta plantilla.

4. Publique la nueva plantilla en la CA.

> [!Note]
> Asegúrese de que la configuración de compatibilidad de la plantilla está establecida en **Windows server 2012 R2** , ya que hay un problema conocido en el que las plantillas no están visibles si la compatibilidad está establecida en windows Server 2016 o una versión posterior. Para obtener más información, consulte [no puede seleccionar plantillas de certificados compatibles con CA de Windows server 2016 desde servidores de CA o de Windows server 2016 o versiones posteriores ](https://support.microsoft.com/en-in/help/4508802/cannot-select-certificate-templates-in-windows-server-2016).


#### <a name="configure-the-cepces01-instance"></a>Configuración de la instancia de CEPCES01

##### <a name="step-1-install-the-instance"></a>Paso 1: instalar la instancia

Para instalar la instancia de CEPCES01, use cualquiera de los métodos siguientes.

**Método 1**

Consulte los artículos siguientes para obtener una guía paso a paso para habilitar el procesamiento de eventos complejos y CES para la autenticación de nombre de usuario y contraseña:

[Guía de Servicio web de directiva de inscripción de certificados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831625(v=ws.11))

[Guía de Servicio web de inscripción de certificados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11)#configure-a-ca-for-the-certificate-enrollment-web-service)

> [!Note]
> Asegúrese de no seleccionar la opción "habilitar renovación basada en claves" Si configura las instancias de CEP y CES de la autenticación de nombre de usuario y contraseña.

**Método 2**

Puede usar los siguientes cmdlets de PowerShell para instalar las instancias de CEP y CES:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature Adcs-Enroll-Web-Pol
Add-WindowsFeature Adcs-Enroll-Web-Svc
```

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Username -SSLCertThumbprint "sslCertThumbPrint"
```

Este comando instala el Servicio web de directiva de inscripción de certificados (CEP) especificando que se usa un nombre de usuario y una contraseña para la autenticación. 

> [!Note]
> En este comando, \<**SSLCertThumbPrint**\> es la huella digital del certificado que se usará para enlazar IIS.

```PowerShell
Install-AdcsEnrollmentWebService -ApplicationPoolIdentity -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Username
```

Este comando instala el Servicio web de inscripción de certificados (CES) para usar la entidad de certificación para un nombre de equipo de **CA1.contoso.com** y un nombre común de CA de **contoso-CA1-CA**. La identidad de CES se especifica como la identidad predeterminada del grupo de aplicaciones. El tipo de autenticación es **username**. SSLCertThumbPrint es la huella digital del certificado que se usará para enlazar IIS.

##### <a name="step-2-check-the-internet-information-services-iis-manager-console"></a>Paso 2 comprobar la consola del administrador de Internet Information Services (IIS)

Después de una instalación correcta, espera ver la siguiente pantalla en la consola del administrador de Internet Information Services (IIS).
![el](media/certificate-enrollment-certificate-key-based-renewal-4.png) del administrador de IIS 

En **sitio web predeterminado**, seleccione **ADPolicyProvider_CEP_UsernamePassword**y, a continuación, abra configuración de la **aplicación**. Anote el **identificador** y el **URI**.

Puede Agregar un **nombre descriptivo** para la administración.

#### <a name="configure-the-cepces02-instance"></a>Configuración de la instancia de CEPCES02

##### <a name="step-1-install-the-cep-and-ces-for-key-based-renewal-on-the-same-server"></a>Paso 1: instalar el procesamiento de eventos complejos y CES para la renovación basada en claves en el mismo servidor. 

Ejecute el siguiente comando en PowerShell:

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Certificate -SSLCertThumbprint "sslCertThumbPrint" -KeyBasedRenewal
```

Este comando instala el Servicio web de directiva de inscripción de certificados (CEP) y especifica que se usa un certificado para la autenticación. 

> [!Note]
> En este comando, \<SSLCertThumbPrint\> es la huella digital del certificado que se usará para enlazar IIS. 

La renovación basada en claves permite a los clientes de certificados renovar sus certificados con la clave de su certificado existente para la autenticación. En el modo de renovación basada en claves, el servicio solo devolverá las plantillas de certificado que se hayan establecido para la renovación basada en claves.

```PowerShell
Install-AdcsEnrollmentWebService -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Certificate -ServiceAccountName "Contoso\cepcessvc" -ServiceAccountPassword (read-host "Set user password" -assecurestring) -RenewalOnly -AllowKeyBasedRenewal
```

Este comando instala el Servicio web de inscripción de certificados (CES) para usar la entidad de certificación para un nombre de equipo de **CA1.contoso.com** y un nombre común de CA de **contoso-CA1-CA**. 

En este comando, la identidad del Servicio web de inscripción de certificados se especifica como la cuenta de servicio **cepcessvc** . El tipo de autenticación es **Certificate**. **SSLCertThumbPrint** es la huella digital del certificado que se usará para enlazar IIS.

El cmdlet **RenewalOnly** permite que CES se ejecute en modo de solo renovación. El cmdlet **AllowKeyBasedRenewal** también especifica que el CES aceptará las solicitudes de renovación basadas en claves para el servidor de inscripción. Se trata de certificados de cliente válidos para la autenticación que no se asignan directamente a una entidad de seguridad.

> [!Note]
> La cuenta de servicio debe formar parte del grupo **IISUsers** en el servidor.

##### <a name="step-2-check-the-iis-manager-console"></a>Paso 2 comprobar la consola del administrador de IIS

Después de una instalación correcta, espera ver la siguiente pantalla en la consola del administrador de IIS.
![el](media/certificate-enrollment-certificate-key-based-renewal-5.png) del administrador de IIS 

Seleccione **KeyBasedRenewal_ADPolicyProvider_CEP_Certificate** en **sitio web predeterminado** y Abra **configuración**de la aplicación. Anote el **identificador** y el **URI**. Puede Agregar un **nombre descriptivo** para la administración.

> [!Note]
> Si la instancia de se instala en un nuevo servidor, compruebe el identificador para asegurarse de que el identificador sea el mismo que se generó en la instancia de CEPCES01. Puede copiar y pegar el valor directamente si es diferente.

#### <a name="complete-certificate-enrollment-web-services-configuration"></a>Completar la configuración de servicios Web de inscripción de certificados

Para poder inscribir el certificado en nombre de la funcionalidad de CEP y CES, tiene que configurar la cuenta de equipo del grupo de trabajo en Active Directory y, a continuación, configurar la delegación restringida en la cuenta de servicio.

##### <a name="step-1-create-a-computer-account-of-the-workgroup-computer-in-active-directory"></a>Paso 1: crear una cuenta de equipo del equipo de grupo de trabajo en Active Directory

Esta cuenta se usará para la autenticación para la renovación basada en claves y la opción "publicar en Active Directory" en la plantilla de certificado.

> [!Note]
> No es necesario unir el equipo cliente a un dominio. Esta cuenta entra en imagen mientras realiza la autenticación basada en certificados en KBR para el servicio dsmapper.

![Nuevo objeto](media/certificate-enrollment-certificate-key-based-renewal-6.png) 
 
##### <a name="step-2-configure-the-service-account-for-constrained-delegation-s4u2self"></a>Paso 2: configurar la cuenta de servicio para la delegación restringida (S4U2Self)

Ejecute el siguiente comando de PowerShell para habilitar la delegación restringida (S4U2Self o cualquier protocolo de autenticación):

```PowerShell
Get-ADUser -Identity cepcessvc | Set-ADAccountControl -TrustedToAuthForDelegation $True
Set-ADUser -Identity cepcessvc -Add @{'msDS-AllowedToDelegateTo'=@('HOST/CA1.contoso.com','RPCSS/CA1.contoso.com')}
```

> [!Note]
> En este comando, \<cepcessvc\> es la cuenta de servicio y < CA1. contoso. com > es la entidad de certificación.

> [!Important]
> No estamos habilitando la marca RENEWALONBEHALOF en la CA en esta configuración porque usamos la delegación restringida para realizar el mismo trabajo. Esto nos permite evitar agregar el permiso para la cuenta de servicio a la seguridad de la entidad de certificación.

##### <a name="step-3-configure-a-custom-port-on-the-iis-web-server"></a>Paso 3: configurar un puerto personalizado en el servidor Web de IIS

1. En la consola del administrador de IIS, seleccione sitio web predeterminado.

2. En el panel acción, seleccione Editar enlace de sitio. 

3. Cambie la configuración de Puerto predeterminada de 443 a su puerto personalizado. La captura de pantalla de ejemplo muestra un valor de puerto de 49999.
   ![cambiar el puerto](media/certificate-enrollment-certificate-key-based-renewal-7.png) 

##### <a name="step-4-edit-the-ca-enrollment-services-object-on-active-directory"></a>Paso 4: editar el objeto de servicios de inscripción de CA en Active Directory

1. En un controlador de dominio, abra ADSIEdit. msc.

2. [Conéctese a la partición de configuración](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/ff730188(v=ws.10))y navegue hasta el objeto de servicios de inscripción de CA:
   
   CN = ENTCA, CN = Servicios de inscripción, CN = Public Key Services, CN = Services, CN = Configuration, DC = Contoso, DC = com

3. Haga clic con el botón derecho en el objeto CA y edítelo. Cambie el atributo **atributo mspki-Enrollment-** Servers mediante el puerto personalizado con los URI de servidor de procesamiento de eventos complejos y CES encontrados en la configuración de la aplicación. Por ejemplo:

   ```
   140https://cepces.contoso.com:49999/ENTCA_CES_UsernamePassword/service.svc/CES0   
   181https://cepces.contoso.com:49999/ENTCA_CES_Certificate/service.svc/CES1
   ```
   
   ![Editor ADSI](media/certificate-enrollment-certificate-key-based-renewal-8.png) 

#### <a name="configure-the-client-computer"></a>Configurar el equipo cliente

En el equipo cliente, configure las directivas de inscripción y la Directiva de inscripción automática. Para ello, sigue estos pasos:

1. Seleccione **inicio** > **Ejecutar**y, a continuación, escriba **gpedit. msc**.

2. Vaya a **configuración del equipo** > configuración de **Windows** > **configuración de seguridad**y, a continuación, haga clic en **directivas de clave pública**.

3. Habilite la **Directiva cliente de servicios de certificados-inscripción automática** para que coincida con la configuración de la siguiente captura de pantalla.
   ![](media/certificate-enrollment-certificate-key-based-renewal-9.png) de directiva de grupo de certificados
 
4. Habilite **la Directiva de inscripción de certificados de cliente de servicios de Certificate Server**.

   a. Haga clic en **Agregar** para agregar la Directiva de inscripción y escriba el URI de procesamiento de eventos complejos con **UsernamePassword** que se editó en ADSI.
   
   b. En **tipo de autenticación**, seleccione **nombre de usuario y contraseña**.
   
   c. Establezca una prioridad de **10**y, a continuación, valide el servidor de directivas.
      ![Directiva de inscripción](media/certificate-enrollment-certificate-key-based-renewal-10.png)

   > [!Note]
   > Asegúrese de que el número de puerto se agrega al URI y se permite en el firewall.

5. Inscriba el primer certificado del equipo a través de certlm. msc.
   ![Directiva de inscripción](media/certificate-enrollment-certificate-key-based-renewal-11.png)

   Seleccione la plantilla KBR e inscriba el certificado.
   ![Directiva de inscripción](media/certificate-enrollment-certificate-key-based-renewal-12.png)

6. Abra **gpedit. msc** de nuevo. Edite el **cliente de servicios de certificados: Directiva de inscripción de certificados**y, a continuación, agregue la Directiva de inscripción de la renovación basada en claves:

   a. Haga clic en **Agregar**, escriba el URI de procesamiento de eventos complejos con el **certificado** que se editó en ADSI. 
   
   b. Establezca una prioridad de **1**y, a continuación, valide el servidor de directivas. Se le pedirá que realice la autenticación y elija el certificado que inscribió inicialmente.

   ![Directiva de inscripción](media/certificate-enrollment-certificate-key-based-renewal-13.png) 

> [!Note]
> Asegúrese de que el valor de prioridad de la Directiva de inscripción de la renovación basada en claves es inferior a la prioridad de la prioridad de la Directiva de inscripción de contraseña de nombre de usuario. La primera preferencia se da a la prioridad más baja.

## <a name="testing-the-setup"></a>Probar la configuración

Para asegurarse de que la renovación automática funciona, debe renovar el certificado con la misma clave mediante MMC para comprobar la renovación manual. Además, se le pedirá que seleccione un certificado mientras se renueva. Puede elegir el certificado que se inscribió anteriormente. Se espera el símbolo del sistema.

Abra el almacén de certificados personal del equipo y agregue la vista "certificados archivados". Para ello, agregue el complemento de cuenta de equipo local a MMC. exe, resalte **certificados (equipo local)** . para ello, haga clic en él, haga clic en **Ver** en la **pestaña acción** situada a la derecha o en la parte superior de MMC, haga clic en **Ver opciones**, seleccione **certificados archivados**y, a continuación, haga clic en **Aceptar**.

### <a name="method-1"></a>Método 1 

Ejecuta el siguiente comando:

```PowerShell
certreq -machine -q -enroll -cert <thumbprint> renew
```

![.](media/certificate-enrollment-certificate-key-based-renewal-14.png)

### <a name="method-2"></a>Método 2

Avance la hora y la fecha del equipo cliente a la hora de renovación de la plantilla de certificado.

Por ejemplo, la plantilla de certificado tiene una configuración de validez de 2 días y una configuración de renovación de 8 horas configurada. El certificado de ejemplo se emitió a las 4:00 A.M. el día 18 del mes expira a las 4:00 A.M. el 20. El motor de inscripción automática se desencadena al reiniciar y en cada intervalo de 8 horas (aproximadamente).

Por lo tanto, si avanza el tiempo hasta 8:10 P.M. el 19 desde que nuestra ventana de renovación se estableció en 8 horas en la plantilla, la ejecución de certutil-Pulse (para desencadenar el motor de AE) inscribe el certificado automáticamente.

![.](media/certificate-enrollment-certificate-key-based-renewal-15.png)
 
Una vez finalizada la prueba, revierta la configuración de tiempo al valor original y, a continuación, reinicie el equipo cliente.

> [!Note]
> La captura de pantalla anterior es un ejemplo que muestra que el motor de inscripción automática funciona según lo esperado porque la fecha de la entidad de certificación todavía está establecida en el decimoctavo. Por lo tanto, sigue emitiendo certificados. En una situación real, esta gran cantidad de renovaciones no se producirán.

## <a name="references"></a>Referencias

[Guía del laboratorio de pruebas: demostrar la renovación basada en claves del certificado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj590165(v%3Dws.11))

[Servicios Web de inscripción de certificados](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Certificate-Enrollment-Web-Services/ba-p/397385)

[Install-AdcsEnrollmentPolicyWebService](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcsenrollmentpolicywebservice?view=win10-ps)

[Install-AdcsEnrollmentWebService](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcsenrollmentwebservice?view=win10-ps)

Consulta también

[Foro de seguridad de Windows Server](https://aka.ms/adcsforum)

[Active Directory preguntas más frecuentes (p + f) sobre la infraestructura de clave pública (PKI) de los servicios de Certificate Server (AD CS)](https://aka.ms/adcsfaq)

[Biblioteca y referencia de documentación de PKI de Windows](https://social.technet.microsoft.com/wiki/contents/articles/987.windows-pki-documentation-reference-and-library.aspx)

[Blog de Windows PKI](https://blogs.technet.com/b/pki/)

[Configuración de la delegación restringida de Kerberos (solo S4U2Proxy o Kerberos) en una cuenta de servicio personalizada para páginas de proxy de inscripción Web](https://support.microsoft.com/help/4494313/configuring-web-enrollment-proxy-for-s4u2proxy-constrained-delegation)