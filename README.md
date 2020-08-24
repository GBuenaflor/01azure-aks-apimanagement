----------------------------------------------------------
# Azure Kubernetes Services (AKS) - Part 08
# Use Azure API Management with microservices (WCF and Web API) deployed in AKS
 
 
#### High Level Architecture Diagram:


![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API02A.png)


#### Configuration Flow :

1. Create Infra using Azure Terraform
2. Generate Lets Encrypt Certificate using CERTBot
3. Add new "TXT" record to Azure DNS Zone
4. Upload Certificate to Application Load Balancer

#### Episode 1 - Build the infrastructure using Azure Terraform and Generate the Lets Encrypt Certificate

![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API-E1-01.png)

----------------------------------------------------------
## 1. Provision the environment using Azure Terraform

```
terraform init
terraform plan
terraform apply
```
----------------------------------------------------------
## 2. Generate Lets Encrypt Certificate using CERTBot, use your Linux Ubuntu Machine



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
sudo ./certbot-auto certonly --manual --preferred-challenges=dns --email gbuenaflor@iom.int --server https://acme-v02.api.letsencrypt.org/directory --agree-tos -d *.aks01-web.iomdev.net
```


 ![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API-E1-02.png)



#### 2.3 Convert the .pem to .pfx

```
sudo su root
cd /etc/letsencrypt/live/aks01-web.iomdev.net
```

#### 2.4 Export certificates

```
# openssl pkcs12 -export -out abc.pfx -inkey privkey.pem -in fullchain.pem
openssl pkcs12 -export -out cert01.pfx -inkey privkey.pem -in fullchain.pem

Enter a Password : [Enter Your Password Here]

```

----------------------------------------------------------
## 3. Add new "TXT" record to Azure DNS Zone

#### Name
```
_acme-challenge.aks01-web.iomdev.net with the following value:
```

#### Value
```
B4lrT50H2kztfTZGKvdQFOemecgfIYSKibahhnhCpfk
```

#### Get the Before continuing, verify the record is deployed.


 ![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API-E1-03.png)
 
----------------------------------------------------------
## 4. Upload the Lets Encrypt Certificate to App Gateway, In Prod you may need to purchase shiny certificate


 ![Image description](https://github.com/GBuenaflor/01azure-aks-apimanagement/blob/master/Images/GB-AKS-API-E1-04.png)


------------------------------------------------------------------------------
 
  

#### Go to other Episodes:

Episode 1 - Build the infrastructure using Azure Terraform and Generate the Lets Encrypt Certificate
Episode 2 - Create ASP.Net Core Web API and WCF app then deploy to AKS using Windows and Linux Node Pool
Episode 3 - Configure API Management External and Internal Enpoints 


 
Link to other Microsoft Azure projects
https://github.com/GBuenaflor/01azure
 


Note: My Favorite -> Microsoft :D
