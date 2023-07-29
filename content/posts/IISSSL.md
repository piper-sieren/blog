+++
author = "Piper Williams"
title = "Create an SSL Certificate for IIS 10"
date = "2023-07-28T14:21:21-07:00"
description = "Create an SSL Certificate for IIS 10"
tags = [
    "Windows",
    "SSL",
    "OpenSSL",
]
categories = [
    "Windows",
    "IIS",
]
series = ["Windows IIS"]
+++

## Create an SSL Certificate for IIS 10 

#### Generate the .csr from IIS GUI
1. Navigate to your site's home and select on 'Server Certificates' located under **ISS**
2. In the far right **Actions** pane, select 'Create Certificate Request' 
3. Fill out the **Distinguished Name Properties** form. This includes the following fields: Common name, Organization, Organizational unit, City/locality, State/province, and Country/region. Select Next
4. Leave the **Cryptographic service provider** and **Bit length** as default, then select Next
5. Save the .csr file to your local machine

#### Export the file in format .pfx using mcc 
1. Go to **Start** -> **Run** and type 'mmc' to open the **Microsoft Management Console** 
2. From the **MMC** menu bar and select **File** -> **Add/Remove Snap-in**
3. Select the **Add** button and select **Certificates** from the list. Select **Add**
4. Slect the **Computer** Account. Click Next, Close, and Ok. 
5. Navigate to **Certificate Enrollment Requests** -> **Certificates** 
6. Right click your previously created certificate and select **All Tasks** -> **Export...**  
7. Choose **Export with private key** on the radio button options, select Next.
8. In the **certificate export wizard** select **Personal Information Exchange - PKCS #12 (.PFX)** 
9. Set a password, **Browse** to where you would like to save your export, and click Next, then Finished 

#### Convert .pfx to RSA private key format 
1. Navigate to the downloaded location of the .pfx file using **CMD**
2. Type the following command to first convert the .pfx file to .pem
{{<highlight html>}}
openssl pkcs12 -in filename.pfx -nocerts -out key.pem
{{</highlight>}}
3. Type the following command to convert the .pem file to .key
{{<highlight html>}}
openssl rsa -in key.pem -out myserver.key
{{</highlight>}}

#### Upload your Certificate Signing Request and Private Key to your preverd Certificate Authority (CA)

#### Download Response from CA
1. Download the .crt file generated as a response from the Certificate Authority (CA)
2. Navigate to the file path inside your **CMD** and use the following to convert the files to a .pfx format
{{<highlight html>}}
openssl pkcs12 -export-certpbe PBE-SHA1-3DES -keypbe PBE-SHA1-3DES -nomac -inkey myserver.key -in downloaded.crt -out example.com.pfx
{{</highlight>}}

#### Upload to IIS 
1. Navigate to your site's home and select on 'Server Certificates' located under **ISS**
2. In the far right **Actions** pane, select 'Complete Certificate Signing Request'
3. Upload your created .pks file 

#### Site Bindings
1. Navigate to your desired IIS Site and select **Bindings** in the far right **Actions** pane
2. Select **Add** 
3. Fill out **Add site bindings** with the following: https, All Unassigned, port 443, and choose your certificate from the dropdown menu. Select OK 


