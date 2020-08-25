----------------------------------------------------------
# Azure Kubernetes Services (AKS) - Part 08
# Use Azure API Management with microservices (WCF and Web API) deployed in AKS - Episode 01
 
 
#### High Level Architecture Diagram:


![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API02B.png)

----------------------------------------------------------

#### Episode 1 - Build the infrastructure using Azure Terraform and Generate the Lets Encrypt Certificate


1. Create Infra using Azure Terraform
2. Generate Lets Encrypt Certificate using certbot,and add new "TXT" record to Azure DNS Zone
3. Convert the certificate .pem to .pfx format 
4. Upload Certificate to Application Load Balancer


![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API-E1-01.png)

----------------------------------------------------------
### 1. Provision the environment using Azure Terraform

```
terraform init
terraform plan
terraform apply
```
----------------------------------------------------------
### 2. Generate Lets Encrypt Certificate using certbot,and add new "TXT" record to Azure DNS Zone


#### 2.1 install certbot 

```
git clone https://github.com/certbot/certbot.git
cd certbot && ./certbot-auto
sudo apt-get install letsencrypt

```

#### 2.2 Generate a wild Card Certificate
 
 ```
sudo chown root ./certbot-auto
sudo chmod 0755 ./certbot-auto
sudo ./certbot-auto certonly --manual --preferred-challenges=dns --email <YourEmailHere> --server https://acme-v02.api.letsencrypt.org/directory --agree-tos -d *.aks01-web.xxxxxxx.net
```

#### 2.3 Add new "TXT" record to Azure DNS Zone

#### Name
```
_acme-challenge.aks01-web.xxxxxxx.net with the following value:
```

#### Value
```
B4lrT50H2kztfTZGKvdQFOemecgfIYSKibahhnhCpfk
```


#### Before continuing, verify the record is deployed.

 ![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API-E1-02.png)


----------------------------------------------------------
### 3. Convert the certificate .pem to .pfx format 

#### 3.1 Map to the certificate location

```
sudo su root
cd /etc/letsencrypt/live/aks01-web.xxxxxxx.net  
```

#### 3.2 Export the certificate
```
openssl pkcs12 -export -out cert01.pfx -inkey privkey.pem -in fullchain.pem

Enter a Password : [CertificatePassword]
```
 
 
----------------------------------------------------------
### 4. Upload the Lets Encrypt Certificate to App Gateway, For testing add new Listener and backend pool connected to a VM with Default IIS configurations. In Production you may need to purchase shiny certificate.


 ![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API-E1-03.png)


------------------------------------------------------------------------------
 
  

#### Go to next or other Episodes:
> Episode1 - Build the infrastructure using Azure Terraform and Generate the Lets Encrypt Certificate 

[Episode2](https://github.com/GBuenaflor/01azure-aks-apimanagement-02/) - Create and contenerize ASP.Net Core Web API and WCF app then deploy to AKS ( Windows and Linux Node Pool)

[Episode3](https://github.com/GBuenaflor/01azure-aks-apimanagement-03/) - Configure API Management External and Internal Enpoints



------------------------------------------------------------------------------
 
Link to other Microsoft Azure projects
https://github.com/GBuenaflor/01azure
  
Note: My Favorite -> Microsoft :D
