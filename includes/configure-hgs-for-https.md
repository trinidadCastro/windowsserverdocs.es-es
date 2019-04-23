En primer lugar, obtener un certificado SSL de HGS de la entidad emisora de certificados. Cada equipo host debe confiar en el certificado SSL, por lo que se recomienda que emitir el certificado SSL de su empresa pública clave infraestructura o terceros CA. Cualquier certificado SSL admitido por IIS es compatible con HGS, sin embargo **el nombre de sujeto del certificado debe coincidir con el nombre completo del servicio HGS** (nombre de red distribuida de clúster). Por ejemplo, si el dominio HGS es "bastion.local" y el nombre del servicio HGS es "hgs", se debe emitir el certificado SSL "hgs.bastion.local". Puede agregar nombres DNS adicionales para el campo de nombre alternativo de sujeto del certificado si es necesario.

Una vez que tiene el protocolo SSL de certificado, abra una sesión de PowerShell con privilegios elevados y proporcione la ruta de acceso al certificado cuando ejecute [conjunto HgsServer](https://technet.microsoft.com/itpro/powershell/windows/host-guardian-service/server/set-hgsserver):


```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

O bien, si ya ha instalado el certificado en el almacén de certificados local, puede hacer referencia mediante la huella digital:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

> [!IMPORTANT]
> Configuración de HGS con un certificado SSL no deshabilita el punto de conexión HTTP.
> Si desea permitir solo el uso del punto de conexión HTTPS, configure Firewall de Windows para bloquear las conexiones entrantes al puerto 80.
> **No modifique los enlaces de IIS** para que sitios Web HGS para quitar el punto de conexión HTTP; no se admite para realizar esta acción.
