---
title: Agregar nombres de dominio de tercer nivel
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5608fb5417b9e958b45d150879daccc3b7767e59
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310234"
---
# <a name="add-third-level-domain-names"></a>Agregar nombres de dominio de tercer nivel

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para habilitar la solicitud de nombres de dominio de tercer nivel por parte de los usuarios, vaya al Asistente de configuración de nombre de dominio. Deberá crear e instalar un ensamblado de código que será utilizado por el Administrador de dominios del sistema operativo.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Cree un proveedor de nombres de dominio de tercer nivel  
 Si desea que los nombres de dominio de tercer nivel estén disponibles, cree e instale un ensamblado de código que proporcione los nombres de dominio al asistente. Para ello deberá completar las tareas siguientes:  
  
-   [Agregar una implementación de la interfaz IDomainSignupProvider al ensamblado](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Agregar una implementación de la interfaz IDomainMaintenanceProvider al ensamblado](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Firmar el ensamblado con una firma Authenticode](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Instalar el ensamblado en el equipo de referencia](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Reiniciar el servicio de administración de nombres de dominio de Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="add-an-implementation-of-the-idomainsignupprovider-interface-to-the-assembly"></a><a name="BKMK_DomainSignup"></a>Agregar una implementación de la interfaz IDomainSignupProvider al ensamblado  
 La interfaz de IDomainSignupProvider se utiliza para agregar ofertas de dominios al asistente.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Para agregar el código de IDomainSignupProvider al ensamblado  
  
1.  Abra Visual Studio 2008 como administrador; para ello, haga clic con el botón derecho en el menú **Inicio** y seleccione **Ejecutar como administrador**.  
  
2.  Haga clic en **Archivo**, **Nuevo** y a continuación haga clic en **Proyecto**.  
  
3.  En el cuadro de diálogo **Nuevo proyecto**, haga clic en **Visual C#** , **Biblioteca de clases**, escriba un nombre para la solución y a continuación haga clic en **Aceptar**.  
  
4.  Cambie el nombre del archivo Class1.cs. Por ejemplo, MyDomainNameProvider.cs  
  
5.  Agregue referencias a los archivos Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll y WssgCommon.dll.  
  
6.  Agregue lo siguiente mediante declaraciones.  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Cambie el espacio de nombres y el encabezado de clase para que coincida con el siguiente ejemplo.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Agregue el método Inicializar y las variables necesarias a la clase que define las ofertas que se incluirán en el asistente.  
  
    > [!NOTE]
    >  El método Inicializar define un identificador para el proveedor de dominios que debe ser único. Un modo sencillo de realizar esto es definir un GUID como el identificador. Para obtener más información sobre cómo crear un GUID, consulte [Crear GUID (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
     En el siguiente ejemplo de código se muestra el método Inicialize.  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. Agregue el método GetOfferings, que genera una lista de las ofertas inicializadas en el paso anterior. En el siguiente ejemplo de código se muestra el método GetOfferings.  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. Agregue el método FindOfferingForDomain, que devuelve la oferta de la lista. En el siguiente ejemplo de código se muestra el método FindOfferingForDomain.  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. Agregue el método SetCredentials, que define las credenciales necesarias para acceder a las ofertas. En el siguiente ejemplo de código se muestra el método SetCredentials.  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. Agregue el método ValidateCredentials, que valida las credenciales definidas por SetCredentials. En el siguiente ejemplo de código se muestra el método ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. Agregue el método GetAvailableDomainRoots, que genera la lista de nombres de dominio raíz compatibles con la oferta especificada en la solicitud. La lista de nombres de dominio raíz no puede estar vacía. En el siguiente ejemplo de código se muestra el método GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Agregue el método GetUserDomainNames, que genera una lista con los nombres de dominio del usuario actual en relación con la oferta actual. La lista puede estar vacía. En el siguiente ejemplo de código se muestra el método  GetUserDomainNames.  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. Agregue el método GetUserDomainQuota, que devuelve el número máximo de dominios permitidos por la oferta especial. Si no es aplicable un máximo, este método debería devolver 0. En el siguiente ejemplo se muestra el método GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Agregue el método CheckDomainAvailability, que comprueba la disponibilidad del nombre de dominio solicitado y puede generar una lista de sugerencias. En el siguiente ejemplo de código se muestra el método CheckDomainAvailability.  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Agregue el método CommitDomain, que confirma el nombre de dominio solicitado. Si se completa este método correctamente, el nombre de dominio se asociará con la cuenta de usuario, el proveedor de mantenimiento será avisado inmediatamente para que recupere el certificado si el estado es FullyOperational y el proveedor y la oferta se activarán. En el siguiente ejemplo de código se muestra el método CommitDomain.  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. Agrega el método ReleaseDomain, que informa al proveedor de que el usuario desea liberar el nombre de dominio. En el siguiente ejemplo de código se muestra el método ReleaseDomain.  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. Agrega el método GetProviderLandingUrl, que genera la dirección URL de la página de aterrizaje en el flujo de trabajo de contratación de dominio. En el siguiente ejemplo de código se muestra el método GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Agrega el método GetDomainMaintenanceProvider, que genera una instancia de IDomainMaintenanceProvider que se utiliza para tareas de mantenimiento del dominio. Este método se convoca después de la finalización correcta del método CommitDomain y durante el inicio del Administrador de dominios. En el siguiente ejemplo de código se muestra el método GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Guarde el proyecto y no lo cierre, ya que lo agregará en el procedimiento siguiente. No podrá generar el proyecto hasta que complete el procedimiento siguiente.  
  
###  <a name="add-an-implementation-of-the-idomainmaintenanceprovider-interface-to-the-assembly"></a><a name="BKMK_DomainMaintenance"></a>Agregar una implementación de la interfaz IDomainMaintenanceProvider al ensamblado  
 IDomainMaintenanceProvider se utiliza para mantener el dominio después de su creación.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Para agregar el código de IDomainMaintenanceProvider al ensamblado  
  
1.  Agregue el encabezado de clase al proveedor de mantenimiento de dominio. Asegúrese de que el nombre del proveedor coincida con el nombre del método GetDomainMaintenanceProvider definido anteriormente.  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  Agregue el método Activate que configura el proveedor activo. En el siguiente ejemplo de código se muestra el método Activate.  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  Agregue el método Deactivate, que se utiliza para desactivar todas las acciones. En el siguiente ejemplo de código se muestra el método Deactivate.  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  Agregue el método SetCredentials, que actualiza las credenciales de usuario. Por ejemplo, este método puede llamarse para actualizar credenciales que ya no sean válidas. En el siguiente ejemplo de código se muestra el método SetCredentials.  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  Agregue el método ValidateCredentials, que valida las credenciales especificadas. En el siguiente ejemplo de código se muestra el método ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  Agregue el método GetPublicAddress, que da como resultado la dirección IP externa del servidor. En el siguiente ejemplo de código se muestra el método GetPublicAddress.  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  Agregue el método SubmitCertificateRequest, que envía la solicitud de certificado del nombre de dominio que esté configurando.  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  Agrega el método GetCertificateResponse, que da como resultado la respuesta del certificado si el estado del dominio es FullyOperational. Este método se utiliza tanto para llamadas de nuevas solicitudes de certificado como para solicitudes de renovación. En el siguiente ejemplo de código se muestra el método GetCertificateResponse.  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. Agregue el método SubmitRenewCertificateRequest, que procesa la renovación del certificado. En el siguiente ejemplo de código se muestra el método SubmitRenewCertificateRequest.  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. Agregue el método UpdateDNSRecords, que actualiza los registros DNS guardados por el proveedor. En el siguiente ejemplo de código se muestra el método UpdateDNS.  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. Agregue el método TestConnection, que intenta establecer una conexión con el servicio de backend. Si este método requiere autenticación, se lanzará la excepción correspondiente. En el siguiente ejemplo de código se muestra el método TestConnection.  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. Agregue el método GetDomainState, que da como resultado el estado actual del dominio. En el siguiente ejemplo de código se muestra el método GetDomainState.  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. Agregue el método GetCertificateState, que da como resultado el estado actual del certificado. En el siguiente ejemplo de código se muestra el método GetCertificateState.  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. Guarde y genere la solución.  
  
###  <a name="sign-the-assembly-with-an-authenticode-signature"></a><a name="BKMK_SignAssembly"></a>Firmar el ensamblado con una firma Authenticode  
 Para poder utilizar el ensamblado en el sistema operativo es necesario firmarlo mediante Authenticode. Para obtener más información acerca de cómo firmar el ensamblado, consulte [Signing and Checking Code with Authenticode (Firma y comprobación de código con Authenticode)](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="install-the-assembly-on-the-reference-computer"></a><a name="BKMK_InstallAssembly"></a>Instalar el ensamblado en el equipo de referencia  
 Coloque el ensamblado en un equipo de referencia. Anote la ruta de la carpeta, ya que deberá introducirla en el registro en el paso siguiente.  
  
### <a name="add-a-key-to-the-registry"></a>Agregue una clave al registro  
 La entrada del registro define las características y la ubicación del ensamblado.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Para agregar una clave al registro  
  
1.  En el equipo de referencia. haga clic en **Inicio**, escriba **regedit** y después presione **Entrar**.  
  
2.  En el panel izquierdo, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server**, **Domain Managers** y finalmente **Providers**.  
  
3.  Haga clic con el botón secundario en **Proveedores**, seleccione **Nuevo** y haga clic en **Clave**.  
  
4.  Escriba el identificador del proveedor como el nombre de la clave. El identificador es el GUID definido para el proveedor en el paso 8 de [Agregue una implementación de la interfaz de IDomainSignupProvider al ensamblado](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Haga clic con el botón secundario en la clave que acaba de crear y a continuación haga clic en **Valor de cadena**.  
  
6.  Escriba **Assembly** como el nombre de la cadena y pulse **Entrar**.  
  
7.  Haga clic con el botón secundario sobre la nueva cadena **Assembly** en el panel derecho y a continuación haga clic en **Modificar**.  
  
8.  Escriba la nueva ruta al archivo de ensamblado que haya creado anteriormente y haga clic en **Aceptar**.  
  
9. Vuelva a hacer clic con el botón secundario del ratón y a continuación haga clic en **Valor de cadena**.  
  
10. Escriba **Enabled** como el nombre de la cadena y pulse **Entrar**.  
  
11. Haga clic con el botón secundario sobre la nueva cadena **Enabled** en el panel derecho y a continuación haga clic en **Modificar**.  
  
12. Escriba **True** y haga clic en **Aceptar**.  
  
13. Vuelva a hacer clic con el botón secundario del ratón y a continuación haga clic en **Valor de cadena**.  
  
14. Escriba **Type** como el nombre de la cadena y pulse **Entrar**.  
  
15. Haga clic con el botón secundario sobre la nueva cadena **Type** en el panel derecho y a continuación haga clic en **Modificar**.  
  
16. Escriba el nombre de clase completo del proveedor que se haya definido en el ensamblado y a continuación haga clic en **Aceptar**.  
  
###  <a name="restart-the-windows-server-domain-name-management-service"></a><a name="BKMK_RestartService"></a>Reiniciar el servicio de administración de nombres de dominio de Windows Server  
 Deberá reiniciar el Servicio Windows Server Domain Management para que el proveedor esté disponible en el sistema operativo.  
  
##### <a name="restart-the-service"></a>Reinicio del servicio  
  
1.  Haga clic en **Inicio**, escriba **mmc** y después presione **Entrar**.  
  
2.  Si el complemento Servicios no aparece en la consola, siga los pasos que se indican a continuación para agregarlo:  
  
    1.  Haga clic en **Archivo** y, a continuación, en **Agregar o quitar complemento**.  
  
    2.  En la lista **Complementos disponibles**, haga clic en **Servicios** y, a continuación, en **Agregar**.  
  
    3.  En el cuadro de diálogo **Servicios**, asegúrese de que se haya seleccionado **equipo local** y a continuación haga clic en **Finalizar**.  
  
    4.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Agregar/eliminar complementos**.  
  
3.  Haga doble clic en **Servicios**, desplácese hacia abajo hasta **Windows Server Domain Management** y a continuación haga clic en **Reiniciar el servicio**.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)