# EstagioDevOps
Olá, eu sou Isabelly Nathália e estou participando do processo seletivo para estágio em DevOps. Qualquer dúvida eu me coloco à disposição e  desde já agradeço a oportunidade!
Documentação Terraform

Esta documentação tem como objetivo explicar o funcionamento do código Terraform utilizado para provisionar recursos na AWS. O código foi projetado para criar uma instância EC2 em uma VPC, incluindo as configurações necessárias, como chaves de acesso, grupos de segurança e tabelas de rotas. 

No arquivo “mainOld.tf” está todo o detalhamento do código inicial e as possíveis melhorias. Este arquivo contém descrições sobre todos os recursos, explicando em detalhes o que está sendo feito e, quando necessário, o que pode ser alterado para garantir uma melhor usabilidade e segurança.

As mudanças feitas começam na modularização do código, deixando ele mais dinâmico e fácil de reutilizar. Cada arquivo contém definições detalhadas sobre o que está sendo criado e descreve as novas implementações, a estrutura atual é a seguinte:
• main.tf (raiz): Define o provider e chama os módulos.
•	modules: Contém a subdivisão do código em:
o	    main.tf: Criação dos recursos.
o	    variables.tf: Declaração das variáveis.
o	    outputs.tf: Declaração dos outputs dos dados.

Principais implementações:
  Aumentar a segurança do grupo de segurança: as regras de entrada do grupo de segurança foram alteradas para restringir o acesso. Antes era possível que qualquer IP pudesse acessar a instância, agora com essas alterações, apenas IPs dentro de um intervalo específico (definido conforme necessidade) podem acessar.
  Aumentar zona de disponibilidade: A variável "availability_zones" foi declarada como uma lista, permitindo a inclusão de várias zonas de disponibilidade conforme a necessidade. Na main.tf dentro do módulo, essa mudança resultou na criação de múltiplas subnets, em vez de uma única, com tags individualizadas que utilizam o índice da lista para identificação.  A associação de rotas foi modificada para permitir a associação de todas as subnets existentes, que antes associava apenas uma.
  A declaração de tags no resource “aws_route_table_association” foi retirada por não ser permitida na documentação do Terraform.
  Um filtro foi adicionado na busca da AMI para garantir que apenas máquinas com a arquitetura necessária sejam encontradas. No exemplo, o filtro busca por máquinas com arquitetura de 64 bits. 
  Foi adicionada no script de inicialização a instalação e inicialização automatizada do Nginx. Dessa forma, assim que a instância EC2 for criada, o servidor Nginx será automaticamente configurado.
  Um output acabou sendo removido, pois ele estava comprometendo a segurança do código, esse output servia para exportar a chave privada para acessar a instância EC2, embora esse output estivesse marcado como sensitive para ocultar seu valor, não é recomendado expor informações dessa natureza.
