---
title: Agregar nombres de dominio de tercer nivel
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64bf24e45155fdd981e2061b3de7ebce1c53b36c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-third-level-domain-names"></a>Agregar nombres de dominio de tercer nivel

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes agregar la capacidad de los usuarios solicitar nombres de dominio de tercer nivel en el asistente Configurar nombre de dominio. Para hacer esto, crear e instalar a un ensamblado de código que se usa el Administrador de dominios en el sistema operativo.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Crear un proveedor de nombres de dominio de tercer nivel  
 Para disponer los nombres de dominio de tercer nivel al crear e instalar a un ensamblado de código que proporciona los nombres de dominio al asistente. Para ello, completa las siguientes tareas:  
  
-   [Agregar una implementación de la interfaz IDomainSignupProvider al ensamblado](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Agregar una implementación de la interfaz IDomainMaintenanceProvider al ensamblado](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Firmar el ensamblado con una firma Authenticode](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Instalar al ensamblado en el equipo de referencia](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Reiniciar el servicio de administración de nombre de dominio de Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a>Agregar una implementación de la interfaz IDomainSignupProvider al ensamblado  
 La interfaz IDomainSignupProvider se usa para agregar ofertas de dominio al asistente.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Para agregar el código IDomainSignupProvider al ensamblado  
  
1.  Abre Visual Studio 2008 como administrador haciendo clic en el programa en el **inicio** menú y seleccionando **ejecutar como administrador**.  
  
2.  Haz clic en **archivo**, haz clic en **nueva**y, a continuación, haz clic en **proyecto**.  
  
3.  En la **nuevo proyecto** cuadro de diálogo, haz clic en **Visual C#**, haz clic en **biblioteca de clases**, escribe un nombre para la solución y, a continuación, haz clic en **Aceptar**.  
  
4.  Cambia el nombre del archivo Class1.cs. Por ejemplo, MyDomainNameProvider.cs  
  
5.  Agrega referencias a los archivos Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll y WssgCommon.dll.  
  
6.  Agrega las siguientes instrucciones "using".  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Cambia el espacio de nombres y el encabezado de clase para que coincida con el siguiente ejemplo.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Agrega el método Initialize y las variables necesarias a la clase, que define las ofertas que se presentan en el asistente.  
  
    > [!NOTE]
    >  El método Initialize define un identificador para el proveedor de dominios que debe ser único. Una forma habitual de hacerlo es definir un GUID como identificador. Para obtener más información sobre cómo crear un GUID, consulta [crear Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
     El siguiente ejemplo de código muestra el método Initialize.  
  
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
  
9. Agrega el método GetOfferings, que devuelve la lista de ofertas que se inicializó en el paso anterior. El siguiente ejemplo de código muestra el método GetOfferings.  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. Agrega el método FindOfferingForDomain, que devuelve la oferta desde la lista. El siguiente ejemplo de código muestra el método FindOfferingForDomain.  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. Agrega el método SetCredentials, que define las credenciales necesarias para acceder a las ofertas. El siguiente ejemplo de código muestra el método SetCredentials.  
  
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
  
12. Agrega el método ValidateCredentials, que valida las credenciales definidas por SetCredentials. El siguiente ejemplo de código muestra el método ValidateCredentials.  
  
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
  
13. Agrega el método GetAvailableDomainRoots, que devuelve la lista de nombres de dominio raíz admitidos por la oferta especificada en la solicitud. Esta lista de nombres de dominio raíz no debe estar vacía. El siguiente ejemplo de código muestra el método GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Agrega el método GetUserDomainNames, que devuelve una lista de nombres de dominio que el usuario actual ya posee, en relación con la oferta actual. Esta lista puede estar vacía. El siguiente ejemplo de código muestra el método GetUserDomainNames.  
  
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
  
15. Agrega el método GetUserDomainQuota, que devuelve el número máximo de dominios que permite la oferta especificada. Si no hay un máximo aplicable, este método debería devolver 0. El siguiente ejemplo muestra el método GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Agrega el método CheckDomainAvailability, que comprueba la disponibilidad del nombre de dominio solicitado y puede devolver una lista de sugerencias. El siguiente ejemplo de código muestra el método CheckDomainAvailability.  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Agrega el método CommitDomain, que confirma el nombre de dominio solicitado. Finalización correcta de este método implica que el nombre de dominio se asociará con la cuenta de usuario, se le pedirá inmediatamente al proveedor de mantenimiento que recupere el certificado si el estado es FullyOperational, y el proveedor y la oferta se volverán activos. El siguiente ejemplo de código muestra el método CommitDomain.  
  
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
  
18. Agrega el método ReleaseDomain, que informa al proveedor de que el usuario desea lanzar el nombre de dominio. El siguiente ejemplo de código muestra el método ReleaseDomain.  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. Agrega el método GetProviderLandingUrl, que devuelve la dirección URL de la página de inicio del flujo de trabajo de suscripción de dominio. El siguiente ejemplo de código muestra el método GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Agrega el método GetDomainMaintenanceProvider, que devuelve una instancia de IDomainMaintenanceProvider que se usa para las tareas de mantenimiento del dominio. Este método se llama después de que el método CommitDomain se realiza correctamente, y cuando se inicia el Administrador de dominio. El siguiente ejemplo de código muestra el método GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Guarda el proyecto y no lo cierres porque agregará a él con el siguiente procedimiento. No podrás generar el proyecto hasta que completes el siguiente procedimiento.  
  
###  <a name="BKMK_DomainMaintenance"></a>Agregar una implementación de la interfaz IDomainMaintenanceProvider al ensamblado  
 El código IDomainMaintenanceProvider se usa para mantener el dominio una vez que se ha creado.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Para agregar el código IDomainMaintenanceProvider al ensamblado  
  
1.  Agrega el encabezado de clase para el proveedor de mantenimiento del dominio. Asegúrate de que el nombre que definas para el proveedor coincida con el nombre en el método GetDomainMaintenanceProvider que definiste anteriormente.  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  Agrega el método Activate, que establece el proveedor activo. El siguiente ejemplo de código muestra el método Activate.  
  
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
  
3.  Agrega el método Deactivate, que se usa para desactivar todas las acciones. El siguiente ejemplo de código muestra el método Deactivate.  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  Agrega el método SetCredentials, que actualiza las credenciales de usuario. Por ejemplo, puede llamar a este método para actualizar credenciales que ya no son válidas. El siguiente ejemplo de código muestra el método SetCredentials.  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  Agrega el método ValidateCredentials, que valida las credenciales especificadas. El siguiente ejemplo de código muestra el método ValidateCredentials.  
  
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
  
6.  Agrega el método GetPublicAddress, que devuelve la dirección IP externa del servidor. El siguiente ejemplo de código muestra el método GetPublicAddress.  
  
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
  
7.  Agrega el método SubmitCertificateRequest, que envía la solicitud de certificado para el nombre de dominio configurado actualmente.  
  
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
  
8.  Agrega el método GetCertificateResponse, que devuelve la respuesta de certificado si el estado del dominio es FullyOperational. Este método se llama para ambas solicitudes de nuevo certificado como para las solicitudes de renovación de certificado. El siguiente ejemplo de código muestra el método GetCertificateResponse.  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. Agrega el método SubmitRenewCertificateRequest, que procesa la renovación del certificado. El siguiente ejemplo de código muestra el método SubmitRenewCertificateRequest.  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. Agrega el método UpdateDNSRecords, que actualiza los registros DNS almacenados por el proveedor. El siguiente ejemplo de código muestra el método UpdateDNS.  
  
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
  
11. Agrega el método TestConnection, que intenta establecer una conexión de servicio back-end. Si este método requiere autenticación, debería producirse una excepción apropiada. El siguiente ejemplo de código muestra el método TestConnection.  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. Agrega el método GetDomainState, que devuelve el estado actual del dominio. El siguiente ejemplo de código muestra el método GetDomainState.  
  
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
  
13. Agrega el método GetCertificateState, que devuelve el estado actual del certificado. El siguiente ejemplo de código muestra el método GetCertificateState.  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. Guarda y compila la solución.  
  
###  <a name="BKMK_SignAssembly"></a>Firmar el ensamblado con una firma Authenticode  
 Es necesario que el ensamblado tenga una firma Authenticode para que se use en el sistema operativo. Para obtener más información sobre cómo firmar el ensamblado, consulta [firma y comprobación de código con Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Instalar al ensamblado en el equipo de referencia  
 Coloca el ensamblado en una carpeta en el equipo de referencia. Anota la ruta de acceso de la carpeta porque deberás introducirla en el registro en el siguiente paso.  
  
### <a name="add-a-key-to-the-registry"></a>Agregar una clave al registro  
 Agrega una entrada al registro para definir las características y la ubicación del ensamblado.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Para agregar una clave al registro  
  
1.  En el equipo de referencia, haz clic en **inicio**, escribe **regedit**y, a continuación, presiona **ENTRAR**.  
  
2.  En el panel izquierdo, expande **HKEY_LOCAL_MACHINE**, expande **SOFTWARE**, expande **Microsoft**, expande **Windows Server**, expanda **administradores de dominios**y, a continuación, expande **proveedores**.  
  
3.  Haz clic en **proveedores**, elija **nueva**y, a continuación, haz clic en **clave**.  
  
4.  Escribe el identificador del proveedor como nombre de la clave. El identificador es el GUID que has definido para el proveedor en el paso 8 de [agregar una implementación de la interfaz IDomainSignupProvider al ensamblado](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Haz clic en la clave que has creado y, a continuación, haz clic en **valor de cadena**.  
  
6.  Tipo **ensamblado** el nombre de la cadena y después presiona **ENTRAR**.  
  
7.  Haz clic en el nuevo **ensamblado** de cadena en el panel derecho y, a continuación, haz clic en **modificar**.  
  
8.  Escribe la ruta de acceso completa al archivo de ensamblado que has creado anteriormente y, a continuación, haz clic en **Aceptar**.  
  
9. Haz clic en la clave de nuevo y, a continuación, haz clic en **valor de cadena**.  
  
10. Tipo **habilitado** el nombre de la cadena y después presiona **ENTRAR**.  
  
11. Haz clic en el nuevo **habilitado** de cadena en el panel derecho y, a continuación, haz clic en **modificar**.  
  
12. Tipo **True**y, a continuación, haz clic en **Aceptar**.  
  
13. Haz clic en la clave de nuevo y, a continuación, haz clic en **valor de cadena**.  
  
14. Tipo **tipo** el nombre de la cadena y después presiona **ENTRAR**.  
  
15. Haz clic en el nuevo **tipo** de cadena en el panel derecho y, a continuación, haz clic en **modificar**.  
  
16. Escribe el nombre completo de la clase del proveedor definido en el ensamblado y, a continuación, haz clic en **Aceptar**.  
  
###  <a name="BKMK_RestartService"></a>Reiniciar el servicio de administración de nombre de dominio de Windows Server  
 Debes reiniciar el servicio de administración de dominios de Windows Server para el proveedor de estén disponibles para el sistema operativo.  
  
##### <a name="restart-the-service"></a>Reiniciar el servicio  
  
1.  Haz clic en **inicio**, tipo **mmc**y, a continuación, presiona **ENTRAR**.  
  
2.  Si el complemento Servicios no aparece en la consola, completa los siguientes pasos para agregarlo:  
  
    1.  Haz clic en **archivo**y, a continuación, haz clic en **agregar o quitar complemento**.  
  
    2.  En la **complementos disponibles** la lista, haz clic en **servicios**y, a continuación, haz clic en **agregar**.  
  
    3.  En la **servicios** cuadro de diálogo, asegúrese de que **equipo local** está seleccionado y, a continuación, haz clic en **finalizar**.  
  
    4.  Haz clic en **Aceptar** para cerrar la **agregar o quitar complementos** cuadro de diálogo.  
  
3.  Haz doble clic en **servicios**, desplázate hacia abajo y selecciona **administración de dominios de Windows Server**y, a continuación, haz clic en **reiniciar el servicio**.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)