---------------------------------------------------------
Instalacion de docker
---------------------------------------------------------
sudo apt update

sudo apt install -y ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg \
 | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
 https://download.docker.com/linux/debian \
 $(. /etc/os-release && echo $VERSION_CODENAME) stable" \
 | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


--------------------------------------------------------------
Instalacion de Localstack en debian
--------------------------------------------------------------
docker run -d --name localstack \
  -p 4566:4566 -p 4571:4571 \
  -e SERVICES="s3,lambda,dynamodb,sqs,sns,apigateway,cloudwatch,iam" \
  -e DEBUG=1 \
  localstack/localstack


------------------------------------------------------------------
Instalacion de AWS CLI en debian
------------------------------------------------------------------
sudo apt update

sudo apt install -y unzip curl

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install

aws --version

+ Configuracion de perfil en aws cli
aws configure --profile localstack

+ te pedira estos datos
AWS Access Key ID [None]: test
AWS Secret Access Key [None]: test
Default region name [None]: us-east-1
Default output format [None]: json



-------------------------------------------------
Instalacion de Terraform en debian
-------------------------------------------------
Paso 1 — Agregar la llave GPG de HashiCorp
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

Paso 2 — Agregar el repositorio oficial de Terraform
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
  | sudo tee /etc/apt/sources.list.d/hashicorp.list

Paso 3 — Actualizar repositorios e instalar Terraform
sudo apt update
sudo apt install terraform -y

Paso 4 — Verificar instalación
terraform -version
