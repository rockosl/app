# Passos para implementar code deploy na AWS:

*IAM* - Função para code deoploy EC2
	  - Função EC2 para bucket S3 - Read only

*S3* - Criar bucket para deploy

*EC2* - Ao criar instância definir função IAM de leitura bucket S3 criado anteriormente
	  - Instalar Agente do code deploy na instância (https://docs.aws.amazon.com/pt_br/codedeploy/latest/userguide/codedeploy-agent-operations-install.html)
		
		> Commandos: 
				 1 - sudo yum update
				 2 - sudo yum install ruby
				 3 - sudo yum install wget
				 4 - cd /home/ec2-user
				 5 - wget https:// *NOME BUCKET* .s3. *REGIÃO* .amazonaws.com/latest/install
				   - Referências buckets: https://docs.aws.amazon.com/pt_br/codedeploy/latest/userguide/resource-kit.html#resource-kit-bucket-names
				 6 - chmod +x ./install
				 7 - sudo ./install auto
				 8 - sudo service codedeploy-agent status (para confirmar se está rodando)
				 9 - sudo service codedeploy-agent start (caso não tenha iniciado)


*Code doploy* - Criar aplicativo Plataforma EC2
		  	  - Criar grupo de implantação dentro do aplicativo
				* Selecionar função code deploy criada anteriormente aqui
		  	  - Selecionar Instancias ec2 através das tags ou grupos de auto scaling
		  	  - Pode ser utilizado load balance para não cair a aplicação aqui
		  	  - Pode ser configurado reversão caso dê problema
			
		  	  - Criar arquivo appspec.yml
		  	  - Verificar se apache está rodando se não instalar 
				> comandos: - sudo yum install httpd
				 		    - sudo systemctl start httpd.service
				    		- sudo systemctl enable httpd.service

		 					 - Criar implantação dentro do grupo e selecionar caminho URL do app no bucket S3

*IMPORTANTE* - Caso usado mais de uma instância verificar grupos de auto scaling dentro do grupo da aplicação