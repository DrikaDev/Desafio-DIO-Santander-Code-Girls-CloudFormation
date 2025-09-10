## Desafio CloudFormation - Módulo 10

### Passo 1: Salvar o template
Abrir o VSCode e salvar o template abaixo:

```
Description: Criar um Amazon EC2 simples
Resources:
  MinhaInstancia:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-0ed9277fb7eb570c9
      InstanceType: t2.micro
      Tags :
        - Key: "Name"
          Value: "EC2"      
```

Esse é um template básico para criar um Amazon EC2 simples que serve para entender a estrutura mínima de um template com 
Resources, Properties, Tags.

### Passo 2: 
Faça login na AWS pelo seu usuário IAM  
Vá para o serviço CloudFormation  
Clique em Create stack → With new resources (standard)  
<img width="1867" height="509" alt="image" src="https://github.com/user-attachments/assets/0888e62a-d8d9-4096-b9c9-a382a88b4973" />

### Passo 3: Enviar o template  
Em Specify template, escolha Upload a template file.  
Clique em Choose file e selecione o "EC2.yml".  
Clique em Next.  
<img width="1411" height="531" alt="image" src="https://github.com/user-attachments/assets/3da932db-df4a-4c0c-ada7-087d4e6205d2" />

### Passo 4: Configurar stack  
Stack name → dê um nome, ex: "InstanciaCodeGirls".  
Clique em Next.  
<img width="1868" height="732" alt="image" src="https://github.com/user-attachments/assets/aa4cb800-6077-4734-898e-4a41d665a33f" />

### Passo 5: Configurações adicionais (opcional)  
Tags, permissões IAM, políticas de rollback, etc.  
Para começar, você pode deixar os padrões.  
Clique em Next.  
<img width="1874" height="485" alt="image" src="https://github.com/user-attachments/assets/2216eed1-162a-47e6-9de5-6928e80c2e0a" />

### Passo 6: Criar stack  
Revise as configurações.  
Clique em Submit.  
<img width="1870" height="708" alt="image" src="https://github.com/user-attachments/assets/7b997289-5293-47cc-b578-7d9c3887170b" />

O CloudFormation vai começar a provisionar os recursos.  
Você verá o status CREATE_IN_PROGRESS → depois CREATE_COMPLETE.  
<img width="894" height="199" alt="image" src="https://github.com/user-attachments/assets/0d359275-09c6-4939-8ee8-b3d8cd313de5" />

Agora a instância EC2 foi criada automaticamente com as propriedades definidas no template.

### Passo 7: Verificar a instância
Vá para o serviço EC2 → Instances.
Você verá sua instância com o Nome = EC2 e tipo t2.micro.

### Conclusão
Durante a execução deste lab, encontrei alguns impedimentos relacionados a restrições de acesso a determinadas ferramentas da AWS, 
o que impossibilitou a conclusão completa do exercício.  
Apesar disso, o estudo me permitiu compreender como o CloudFormation funciona, a estrutura de templates, a criação de recursos como 
EC2 e a importância de AMIs válidas por região.  

<img width="804" height="209" alt="image" src="https://github.com/user-attachments/assets/c21f6f2c-e528-4be7-b0f5-797d28ca168a" />
