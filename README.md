## Desafio DIO Santander Code Girls: CloudFormation

Este laboratório tem como objetivo **implementar nossa primeira Stack usando AWS CloudFormation**.  

O **CloudFormation** é um serviço de **Infraestrutura como Código (IaC)** da AWS que permite criar, gerenciar e atualizar recursos automaticamente, como EC2, S3, VPC e RDS, usando **templates em YAML ou JSON**.  

Isso significa que, ao invés de configurar cada serviço manualmente, podemos escrever um “manual de instruções” para a AWS montar toda a infraestrutura para nós, garantindo **consistência, automação e reprodutibilidade**.  

### Vamos então para nosso hands-on e ver a primeira stack funcionando! 💻  

---

## Criação do arquivo .yaml no VSCode:  

   <img width="546" height="81" alt="image" src="https://github.com/user-attachments/assets/88d05bc8-ce8b-4cbf-b81c-3c0debbf6e4b" />  

   Esse template em YAML representa o fluxo criado no desafio anterior:  👉🏻 [Desafio DIO - Santander Code Girls](https://github.com/DrikaDev/Desafio-DIO-Santander-Code-Girls/tree/main)  
   
   <img width="749" height="52" alt="image" src="https://github.com/user-attachments/assets/39115490-e4f4-4e8e-b234-59dab4eb1154" />

### Vamos analisar cada bloco do código

1. Bucket S3  
   Esse bloco cria um bucket S3 chamado ```bucket-dio-santander```
   Sempre que usuário fizer upload de arquivo, o bucket dispara automaticamente um evento para a função Lambda configurada
   (MyLambdaFunction).
   
   <img width="714" height="228" alt="image" src="https://github.com/user-attachments/assets/a5b3ee79-4ecf-49c8-a24d-91f9fefeae87" />

2. Função Lambda  
   Esse bloco cria a função Lambda "MyLambdaProcessFile", em Python 3.9, que será chamada sempre que um upload no S3 acontecer.
   A função usa a Role "LambdaExecutionRole" para acessar os recursos necessários como S3 e EC2.
   
   <img width="723" height="341" alt="image" src="https://github.com/user-attachments/assets/842fb49e-b39e-479d-b093-f33154ab2341" />

3. Permissão do Bucket para invocar a Lambda  
   Esse bloco garante que o bucket S3 criado pode invocar a Lambda "MyLambdaFunction" sempre que houver upload de arquivos.  
   Sem isso, mesmo com o trigger configurado, a Lambda não seria chamada.
   
   <img width="764" height="249" alt="image" src="https://github.com/user-attachments/assets/c8131c79-4f51-4bd3-ab65-7df823b195a0" />

4. Instância EC2  
   Esse bloco cria uma EC2 pequena (t2.micro), com 8 GB de EBS, usando um AMI Amazon Linux.
   A instância é protegida por um Security Group, pronta para receber e processar os dados que a Lambda disparar.
   
   <img width="723" height="318" alt="image" src="https://github.com/user-attachments/assets/e321d700-ef6d-45c1-ada9-ffcda72acc7f" />

5. Volume EBS adicional  
   Esse bloco cria um volume EBS de 20 GB do tipo SSD gp2, na mesma zona de disponibilidade da instância "MyEC2Instance".
   Esse volume poderá ser usado para armazenar dados gerados pelo processamento da instância EC2.
   
   <img width="722" height="181" alt="image" src="https://github.com/user-attachments/assets/f77aa705-0700-4cdd-9a77-968b0b7fe6f7" />

6. Anexação do volume EBS  
   Esse bloco anexa o volume EBS criado (MyEBSVolume) à instância EC2 (MyEC2Instance).  
   O volume se torna utilizável dentro do sistema operacional da instância, acessível pelo dispositivo /dev/sdh.

   <img width="733" height="180" alt="image" src="https://github.com/user-attachments/assets/ad2505cd-4756-40b1-9d73-eccf281e110b" />

7. Security Group para EC2  
   Esse bloco cria um "Security Group" chamado "MyEC2SecurityGroup" com as seguintes regras configuradas:  
   - Pertence à VPC especificada (vpc-12345678).  
   - Permite SSH (porta 22) de qualquer IP.  
   - Permite HTTP (porta 80) de qualquer IP.
     
   Este Security Group depois será associado à instância EC2 para controlar seu acesso externo.

   <img width="721" height="362" alt="image" src="https://github.com/user-attachments/assets/5bd11f42-0588-46d7-a0f9-57c083a0b3ec" />

8. IAM Role para Lambda  
   Esse bloco cria uma "IAM Role" chamada "LambdaExecutionRole", que:  
   - Pode ser assumida pelo serviço Lambda "lambda.amazonaws.com".  
   Permite à Lambda:  
   - Ler e gravar objetos em um bucket S3 específico (MyS3Bucket).  
   - Consultar e enviar comandos a instâncias EC2.  
   
   Isso é essencial para que a Lambda execute tarefas como processar arquivos no S3 e interagir com EC2.

   <img width="721" height="594" alt="image" src="https://github.com/user-attachments/assets/73a7d3c4-c275-48c9-acc3-44a5dea2f48e" />

---

## Criação do CloudFormation no console da AWS:

1. Procure no console da AWS pelo serviço **CloudFormation**  

   <img width="1017" height="242" alt="image" src="https://github.com/user-attachments/assets/757df089-d156-41df-8f26-5dc49dd706dc" />

2. Clique em "Create stack" e selecione "With new resources (standard)  
   
   <img width="1432" height="183" alt="image" src="https://github.com/user-attachments/assets/3678d9d4-e039-4bf6-9352-350744893d65" />

3. Selecione "Choose an existing template"  
   
   <img width="1102" height="241" alt="image" src="https://github.com/user-attachments/assets/625c927e-7d65-416b-a538-1b211bf4e5f4" />

4. Selecione "Upload a template file", faça o upload do arquivo .yaml criado no VSCode e clique em Next:  
   
   <img width="1098" height="467" alt="image" src="https://github.com/user-attachments/assets/d7b897ab-0370-4ea2-836b-00e637acee28" />

5. Dê um nome para a stack e clique em "Next":  
   
   <img width="1414" height="278" alt="image" src="https://github.com/user-attachments/assets/9fb4e465-71c4-49f3-b9f0-ee74a3538260" />

6. Em "Configure stack options" clique no checkbox "**I acknowledge that AWS CloudFormation might create IAM resources.**" e depois clique em "Next":
   
   <img width="1103" height="187" alt="image" src="https://github.com/user-attachments/assets/c362d2b1-116f-482e-ac77-0f7428abe310" />

7. Em "Review and create" reveja os Steps, e clique em "Submit":
   
   <img width="1415" height="491" alt="image" src="https://github.com/user-attachments/assets/0d7f642a-8c2a-4e4c-bac5-c47fefe236a3" />

   <img width="401" height="47" alt="image" src="https://github.com/user-attachments/assets/e9e9de9f-ae54-4990-872d-ad710e1cad0a" />

   <img width="1099" height="175" alt="image" src="https://github.com/user-attachments/assets/69a7b94b-b4bb-430f-af1b-68566e4892f9" />

8. Aguarde até finalizar o progresso da criação:
   
   <img width="301" height="98" alt="image" src="https://github.com/user-attachments/assets/373f7043-400f-48a2-ae89-efe6e5e1e22b" />

O meu deu erro
 ---



