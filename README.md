# Projeto de Migração: Monólito para Monólito Modular

## 1 Visão Geral

Este projeto tem como objetivo realizar a migração de um sistema monolítico tradicional para uma arquitetura de **Monólito Modular (MOM)**. 
A refatoração busca melhorar a escalabilidade, modularidade e manutenibilidade do sistema através da aplicação de princípios como **Domain-Driven Design (DDD)**, **Command Query Responsibility Segregation (CQRS)** e **Arquitetura Limpa (AL)**.

## 2. Arquitetura Escolhida: Comparativo e Justificativas

A migração foi motivada pela necessidade de superar limitações do sistema monolítico tradicional e aproveitar os benefícios de uma estrutura modular, sem abrir mão da simplicidade de deploy que um monólito oferece. A seguir, detalhamos cada arquitetura apresentada no diagrama, destacando as principais diferenças e as justificativas para a mudança.

### 2.1. Monolito Tradicional

- **Descrição:**  
  O sistema original é implementado como um monólito, onde todas as funcionalidades e módulos compartilham o mesmo processo e ambiente de execução.

- **Características e Desafios:**
   - **Acoplamento Forte:**  
     As funcionalidades estão intimamente ligadas, fazendo com que mudanças em um módulo possam impactar o sistema inteiro.
   - **Escalabilidade Limitada:**  
     O sistema só pode ser escalado como um todo, o que pode levar à utilização ineficiente de recursos.
   - **Manutenção Complexa:**  
     Alterações e correções podem se tornar arriscadas e demoradas devido à falta de separação clara entre os domínios.
   - **Deploy Unificado:**  
     Apesar de simplificar o deploy, o fato de todas as funcionalidades estarem no mesmo pacote dificulta a adoção de práticas de atualização contínua sem riscos.

- **Justificativa para a Mudança:**  
  A alta complexidade e os desafios de manutenção e escalabilidade evidenciaram a necessidade de uma separação melhor dos domínios do sistema, reduzindo o risco de impactos colaterais durante alterações.

### 2.2 Monólito Modular (MOM)

- **Descrição:**  
  A nova arquitetura mantém o sistema em um único processo, mas organiza o código em módulos bem definidos, cada um responsável por um domínio específico do negócio.

- **Benefícios:**
   - **Baixo Acoplamento:**  
     Os módulos são independentes e se comunicam por meio de interfaces e eventos, o que minimiza dependências diretas.
   - **Escalabilidade Seletiva:**  
     Permite escalar individualmente os módulos críticos sem precisar replicar o sistema inteiro.
   - **Manutenibilidade Melhorada:**  
     Alterações em um módulo podem ser realizadas com menor impacto nos demais, facilitando testes e atualizações.
   - **Organização e Coesão:**  
     Cada módulo agrupa funcionalidades relacionadas, tornando o sistema mais organizado e alinhado com os princípios do DDD.

- **Justificativa para a Mudança:**  
  Ao adotar o conceito de Monólito Modular, a equipe consegue manter a simplicidade do deploy e a consistência do ambiente monolítico, enquanto aproveita as vantagens da modularização para facilitar a manutenção, permitir escalabilidade seletiva e reduzir riscos durante mudanças.

### 2.3 Comparativo Resumido

| Aspecto               | Monolito Tradicional                                | Monólito Modular (MOM)                          |
| --------------------- | --------------------------------------------------- | ------------------------------------------------ |
| **Acoplamento**       | Alto, com forte interdependência entre funcionalidades | Baixo, com módulos isolados e comunicação definida |
| **Escalabilidade**    | Limitada à replicação do sistema completo           | Seletiva, permitindo escalonamento individual de módulos |
| **Manutenção**        | Complexa e arriscada com mudanças em larga escala     | Mais simples, com testes e atualizações isoladas   |
| **Deploy**            | Unificado, porém impactante em caso de alterações      | Simples e consistente, com possibilidade de deploys segmentados |
| **Organização**       | Estrutura monolítica sem separação clara de domínios    | Organização alinhada com DDD, com responsabilidade bem definida |

### 2.4 Objetivos da Migração

- **Escalabilidade**: Permitir que módulos críticos sejam escalados de forma independente.
- **Modularidade**: Organizar o código e isolar domínios, facilitando manutenções e evoluções.
- **Facilidade de Manutenção**: Reduzir o acoplamento entre funcionalidades e melhorar a testabilidade do sistema.
- **Alta Disponibilidade**: Garantir resiliência e tolerância a falhas.
- **Melhoria no Tempo de Resposta**: Otimizar a performance e reduzir a latência.

### 2.5 Estrutura do Sistema Migrado

O sistema é dividido em módulos, onde cada um atua de forma autônoma dentro do mesmo processo. Exemplos de módulos:
- **Módulo de Produtos**
- **Módulo de Pedidos**
- **Módulo de Atendimento**
- **Módulo de Usuários (Autenticação e Autorização)**

Cada módulo comunica-se por meio de interfaces e, quando necessário, utiliza eventos para interagir com outros módulos.

### 2.6 Conclusão

A transição do monolito tradicional para o Monólito Modular foi realizada para:

- **Reduzir o acoplamento** entre as funcionalidades, evitando que alterações em um módulo afetem o sistema inteiro.
- **Facilitar a escalabilidade**, permitindo que módulos com alta demanda sejam escalados de forma independente.
- **Melhorar a manutenibilidade** do sistema, tornando-o mais organizado e alinhado com os princípios de DDD e Arquitetura Limpa.

A escolha por uma arquitetura modular dentro de um monólito representa um equilíbrio estratégico: mantendo a simplicidade do deploy monolítico e, ao mesmo tempo, ganhando a flexibilidade e robustez de uma estrutura modular.

![Diagrama de Arquitetura do Monólito](analyses/umls/architectures/diagrama-de-arquitetura-monolito.png)
![Diagrama de Arquitetura do Monólito Modular](analyses/umls/architectures/diagrama-de-arquitetura-monolito-modular.png)

## 3. Como identificar os Módulos dentro de um Monólito (para Migração a Monólito Modular)

A identificação de módulos em um sistema monolítico exige analisar o sistema sob múltiplos critérios para encontrar divisões naturais e coesas. Segundo Abgaz et al. (2023) – em sua revisão sistemática sobre decomposição de monólitos – existem quatro abordagens principais de análise do monólito: análise de domínio, análise estática, análise dinâmica e análise de versões ￼. Essas abordagens, frequentemente usadas para identificar microsserviços, também são aplicáveis à modularização interna de um monólito (o monólito modular) ￼. A seguir, detalhamos cada método e os critérios utilizados, incluindo exemplos práticos e ferramentas de apoio quando apropriado.

### Análise de Domínio e Contextos Delimitados (DDD)

A análise de domínio foca em entender a lógica de negócio e os limites naturais dentro do problema que o software resolve. Isso normalmente envolve identificar contextos delimitados (do Domain-Driven Design) – subdomínios ou áreas de negócio coesas que podem se tornar módulos. Abordagens de análise de domínio examinam artefatos de requisitos e design (modelos de casos de uso, diagramas de processos, diagramas ER, etc.) para descobrir “domínios de interesse”, ou seja, temas-chave ou elementos funcionais lógicos do sistema ￼ ￼. Em outras palavras, a partir da documentação e conhecimento do negócio, busca-se dividir o monólito conforme as capacidades de negócio (por exemplo, faturamento, gerenciamento de usuários, catálogo de produtos, etc.). Essas divisões correspondem aos contextos de negócio relativamente independentes. Técnicas do DDD, como event storming e mapeamento de contexto, ajudam os stakeholders a explicitar limites onde o vocabulário e as regras de negócio mudam, indicando módulos potenciais. Por exemplo, em um sistema de comércio eletrônico pode-se identificar contextos como Catálogo de Produtos, Carrinho/Pedidos, Pagamentos e Contas de Usuário, cada qual encapsulando um conjunto distinto de responsabilidades de negócio.

Para realizar essa identificação, é crucial envolver stakeholders (partes interessadas) e especialistas do domínio. Feedback de stakeholders – via workshops, entrevistas e revisão de documentos – é importante para validar se os possíveis módulos fazem sentido do ponto de vista do negócio e dos usuários. De fato, Abgaz et al. observam estudos em que documentação de processos e entrevistas com experts foram usadas junto com diagramas de contexto do domínio para definir os limites iniciais de módulos (contextos) ￼. Esse alinhamento com o conhecimento tácito dos domain experts garante que os módulos identificados correspondam a responsabilidades claras e não quebrem regras de negócio importantes. Ferramentas como o Service Cutter suportam essa fase baseada em domínio: o Service Cutter, por exemplo, utiliza informações de alto nível (entidades de negócio, casos de uso, requisitos de domínio) para sugerir decomposição em serviços/modulos ￼. Em suma, a análise de domínio (muitas vezes chamada model-driven ou domain-driven) identifica módulos candidatos ao redor de Bounded Contexts do domínio, garantindo coesão conceitual desde o início ￼ ￼.

### Análise de Responsabilidades e Funcionalidades

Complementarmente à visão de domínio, analisa-se o monólito sob a ótica de suas responsabilidades e funcionalidades atuais. Aqui, o foco é determinar quais partes do código trabalham juntas para cumprir um determinado serviço ou caso de uso, seguindo o princípio de responsabilidade única. Um módulo adequado deve implementar um conjunto coeso de funcionalidades intimamente relacionadas – por exemplo, todas as funcionalidades relativas a pedidos devem estar no módulo Pedidos. Podemos começar listando as principais funcionalidades ou features do sistema e mapeando quais componentes (classes, funções, componentes UI, tabelas de banco etc.) estão associados a cada funcionalidade. Esse exercício evidencia agrupamentos naturais de elementos de software por responsabilidade.

A análise de responsabilidades muitas vezes revela sub-sistemas lógicos dentro do monólito – por exemplo, em uma aplicação de gestão hospitalar, pode-se ter subconjuntos de funcionalidades de agendamento, prontuário de pacientes, faturamento etc. que já são relativamente separados no código (mesmo que não formalmente modularizados). Critérios como o princípio da responsabilidade única e alto coesão funcional orientam a identificação: classes e funções que colaboram para a mesma tarefa de negócio devem idealmente residir no mesmo módulo. Na prática, pode-se documentar as funções principais do sistema e, a partir delas, inferir possíveis módulos. Essa abordagem utiliza tanto conhecimento de domínio quanto leitura do código para atribuir a cada parte uma responsabilidade primária. Por exemplo, ao revisar um módulo grande de “Processamento de Pedido”, pode-se subdividi-lo em responsabilidades menores: cálculo de frete, cálculo de impostos, autorização de pagamento, etc., e avaliar se essas responsabilidades formam partes coesas ou se algumas deveriam pertencer a módulos separados (como talvez um módulo de Pagamentos separado do módulo de Pedidos).

Vale notar que muitas técnicas de análise estática e de domínio também capturam responsabilidades. Por exemplo, a análise de casos de uso (DomA) identifica funcionalidades e as entidades envolvidas ￼. Já a análise de coesão (discutida adiante) quantitativa pode confirmar se um conjunto de classes realmente apresenta uma única responsabilidade bem definida (alta coesão interna) ￼ ￼. Ferramentas de mapeamento de requisitos para módulos ou catálogos de funcionalidades (por exemplo, rastreabilidade de casos de uso para classes) podem auxiliar nessa etapa. Em essência, a identificação por responsabilidades assegura que cada módulo proposto tenha um propósito claro e delimitado, o que facilita comunicação com times e futuros testes.

### Análise de Dependências Estáticas (Código)

A análise estática examina o código-fonte e suas dependências estruturais para encontrar possíveis divisões modulares. Trata-se de inspecionar chamadas de métodos, importações de classes/pacotes, dependências de bibliotecas e relacionamentos entre componentes no nível de código sem executar o sistema ￼. O objetivo é identificar grupos de classes ou componentes que são fortemente conectados entre si (muitas dependências mútuas) mas fracamente conectados com o resto – esses grupos são candidatos a módulos, pois indicam alta coesão interna e baixo acoplamento externo. Por exemplo, se classes do pacote com.empresa.financeiro referenciam-se frequentemente, mas quase não referenciam classes de com.empresa.vendas, isso sugere que Financeiro e Vendas podem ser módulos separados.

Técnicas da literatura de microsserviços usam extensivamente essa análise estática. Muitas abordagens constroem um grafo de dependências ou de chamadas a partir do código e então aplicam algoritmos de clustering para encontrar comunidades de classes no grafo ￼ ￼. Abgaz et al. apontam que clustering não supervisionado é amplamente usado, incluindo métodos hierárquicos, k-means, propagação de afinidade, detecção de comunidades (Girvan-Newman), entre outros, para agrupar artefatos de software em serviços candidatos ￼. Essencialmente, a matriz ou grafo de dependências serve como base: classes (ou funções/pacotes) são nós, e chamadas ou referências são arestas; algoritmos buscam partições que maximizem conexões intra-módulo e minimizem inter-módulo. Por exemplo, Gysel et al. (Service Cutter) utilizam um conjunto de critérios que incluem dependências de classes e relações de dados para sugerir cortes ￼. Outra ferramenta, Arcan, analisa o código Java para detectar ciclos de dependência e possíveis módulos, sendo usada em pesquisas de extração de microsserviços ￼.

Critérios específicos podem guiar a análise estática manualmente também: camadas do sistema (UI, domínio, persistência) devem preferencialmente não ser módulos separados por si, mas sim cada módulo deve conter suas camadas internas. Busca-se evitar acoplamentos entre possíveis módulos: por exemplo, muitas chamadas entre classes de “Pedidos” e “Inventário” indicam alto acoplamento – talvez esses componentes devam estar no mesmo módulo ou exigir refatorações para reduzir dependências. Dependências cíclicas são sinais de que as fronteiras não estão bem definidas; quebrar ciclos ou agrupá-los dentro de um mesmo módulo é uma tarefa importante. Ferramentas de análise estática (como jDepend, Structure101, SonarQube/Sonargraph ou plugins como ArchUnit para validar regras arquiteturais) ajudam a visualizar esses acoplamentos e a verificar se um particionamento proposto elimina dependências indesejadas entre módulos. Em pesquisas recentes, gráficos de chamada estáticos têm sido combinados com outros dados para melhorar a precisão dos cortes ￼ ￼, mas a análise estática pura já fornece uma base objetiva: o esqueleto estrutural do monólito.

### Análise Dinâmica (Comportamento em Execução)

A análise dinâmica complementa a estática ao observar o sistema em execução, capturando como as partes do monólito interagem de fato em tempo de execução. Enquanto a análise estática considera todas as dependências teóricas no código, a dinâmica revela padrões reais de uso: quais componentes são invocados juntos em cenários reais, frequências de chamadas, sequências de transações, etc. Conforme Abgaz et al. definem, a análise dinâmica envolve examinar propriedades do programa durante sua execução, seja online (instrumentando o código ao rodar) ou coletando logs/traces para análise offline ￼.

Na prática, realiza-se instrumentação (por exemplo, usando AOP ou ferramentas de monitoramento) para registrar traços de execução: chamadas entre classes, tempo de resposta, identificadores de threads, consultas de banco ocorridas, etc. ￼ ￼. Esses dados podem mostrar, por exemplo, que ao executar a função “Finalizar Pedido”, o sistema envolve módulos de Pedido, Pagamento e Notificação em sequência – indicando interações intermodulares necessárias. Se certas componentes quase nunca interagem em tempo real, talvez possam ser separados com segurança; por outro lado, componentes que trocam informação a todo momento podem justificar estarem no mesmo módulo (para evitar sobrecarga de comunicação). A análise de frequência de chamadas também destaca dependências críticas: Abgaz et al. mencionam sobrepor dados dinâmicos em grafos estáticos para ponderar arestas conforme frequência de execução ￼. Assim, uma dependência que é raramente ativada pode ser tratada diferentemente de uma que ocorre em quase todas as transações – este insight ajuda a priorizar cortes que minimizem impacto em desempenho e consistência.

Ferramentas especializadas existem para captura dinâmica. Por exemplo, o Kieker é uma ferramenta que instrumenta aplicações Java para coletar dados de execução e foi empregada em estudos de extração de microsserviços ￼. Monitoramento de logs de acesso ou uso de APMs (Application Performance Management) também fornece dados (p. ex., logs HTTP indicando quais módulos funcionais são invocados para cada endpoint). Em cenários de baixa observabilidade do monólito legado, pode-se habilitar logging detalhado ou temporariamente instrumentar funções-chave. Um caso interessante é usar logs de acesso ao banco de dados: se módulos distintos deveriam ter responsabilidade separada por tabelas distintas, mas os logs mostram um mesmo serviço acessando tabelas de domínios diferentes com frequência, isso indica limites mal desenhados. De fato, uma técnica mencionada em pesquisas é extrair dos logs quais tabelas são usadas por quais funcionalidades (ex.: usando DBeaver para monitorar consultas) ￼ para ajudar a delinear módulos baseados em contexto de dados. Em suma, a análise dinâmica foca no comportamento real e fornece evidências baseadas no uso, evitando que modulagens se baseiem apenas em estrutura estática que talvez não reflita os fluxos de trabalho reais do sistema.

### Análise da Evolução do Código (Histórico de Versões)

Outra perspectiva valiosa é a análise de versões do sistema, examinando o histórico de modificações no código (tipicamente via sistema de controle de versão, como Git). A ideia central é identificar quais partes do código evoluem juntas ao longo do tempo – se dois componentes são frequentemente alterados nos mesmos commits ou releases, isso sugere que têm uma forte relação funcional ou de dependência, podendo ser bons candidatos a ficar no mesmo módulo (para que evoluam em sincronia). Essa técnica revela o acoplamento evolutivo ou lógico que não é óbvio apenas pela estrutura de chamadas.

Abordagens nessa linha coletam dados de commits, diffs e histórico de mudanças para calcular, por exemplo, coeficientes de co-mudança: quantas vezes o arquivo A e B foram editados juntos ￼. Uma alta co-mudança indica que funcionalidades implementadas nesses arquivos estão correlacionadas (talvez uma regra de negócio que exige mudanças coordenadas em múltiplas classes). Assim, para modularizar, é sensato agrupar os itens com alta co-evolução no mesmo módulo, reduzindo a necessidade de mudanças sincronizadas entre módulos distintos. Abgaz et al. relatam estudos onde gráficos estáticos de dependência foram enriquecidos com dados evolutivos – por exemplo, adicionando peso às arestas entre classes proporcionalmente à frequência de commits conjuntos ￼. Esse processo torna o grafo mais representativo das dependências “vivas” no projeto, elevando conexões entre itens que historicamente andam juntos. Em outra abordagem, os dados de versão podem ser analisados separadamente para detectar componentes emergentes: clusters de arquivos que emergem dos padrões de commits.

Exemplo prático: imagine que no monólito de uma loja virtual, toda vez que a funcionalidade de promoção é alterada, também se modificam arquivos da funcionalidade de carrinho. Isso pode indicar que promoção e carrinho estão fortemente ligados (talvez indevidamente) – a análise de versão traria esse acoplamento lógico à tona, orientando o arquiteto a decidir se essas funcionalidades deveriam pertencer a um mesmo módulo ou, ao contrário, se é necessário refatorar para quebrar essa dependência implícita. Ferramentas caseiras podem ser desenvolvidas para extrair tais métricas de repositório (scripts que mineram o log do Git em busca de padrões de co-change). Academicamente, algumas ferramentas também incorporaram essa camada: por exemplo, o Microservice Miner e outros protótipos que integram informações de histórico no algoritmo de decomposição ￼. A análise de evolução garante que a modularização planejada considere a dimensão temporal: módulos bons devem idealmente encapsular também ciclos de manutenção – um módulo deve poder ser mantido ou atualizado sem afetar muitos outros.

### Integração das Análises e Abordagem Híbrida

Cada método acima oferece uma visão diferente do monólito, e na prática a identificação de módulos costuma combinar múltiplas perspectivas para aumentar a confiabilidade. Por exemplo, um processo recomendado é: (i) usar a análise de domínio (e stakeholders) para propor um esboço inicial de módulos (por fronteiras de negócio), (ii) aplicar análise estática para verificar dependências do código que suportam ou contradizem essas fronteiras (ajustando onde necessário para eliminar acoplamentos fortes entre módulos diferentes), (iii) utilizar dados dinâmicos para validar que cada módulo candidato consegue executar suas funções majoritariamente de forma independente (checando se não há chamada excessiva entre módulos em tempo de execução), e (iv) consultar o histórico de versões para confirmar que os arquivos dentro de cada módulo realmente têm alta coesão de mudança e poucas dependências de evolução com módulos externos. Essa abordagem iterativa foi exemplificada em estudos como [P16] (referenciado por Abgaz et al.): os autores primeiro definiram contextos de negócio por documentação e entrevistas, depois refinaram esses bounded contexts utilizando análise estática e dinâmica para ajustar fronteiras ￼. Combinar análise manual (regras definidas por especialistas) com algoritmos de clustering é outra estratégia eficiente – regras baseadas em domínio podem guiar o algoritmo a produzir módulos alinhados com requisitos de alto nível ￼ ￼.

Além disso, é útil aplicar heurísticas conhecidas de design modular: por exemplo, agrupar classes que acessam os mesmos dados e separar classes que mudam por motivos diferentes (princípio de separação de motivos de mudança). Ferramentas de apoio permitem experimentar diferentes cortes. Algumas oferecem visualização dos módulos candidatos – e.g., a ferramenta ExploreViz mencionada por Abgaz et al. gera visualizações 3D dos serviços propostos com base em dados estáticos/dinâmicos ￼, ajudando a equipe a inspecionar se os agrupamentos fazem sentido. Algoritmos genéticos também foram aplicados para otimizar a composição dos módulos de forma multi-objetivo, tentando maximizar coesão e minimizar acoplamento automaticamente ￼ ￼.

Importante destacar que, mesmo com análises automatizadas, a validação humana é imprescindível. Após identificar módulos candidatos com essas técnicas, deve-se revisá-los com arquitetos e desenvolvedores experientes no sistema para verificar se: (a) cada módulo tem um nome e propósito claro, (b) as fronteiras não quebram regras de negócio (por exemplo, um módulo não deveria precisar violar encapsulamento de outro para cumprir suas tarefas), e (c) se a modularização faz sentido para a estrutura da equipe e planos futuros (considerando, por exemplo, quais partes poderiam virar microsserviços independentes no futuro próximo). Esse ciclo de análise e revisão aumenta as chances de identificar módulos de qualidade.

[//]: # (Tabela 1 resume os principais métodos de identificação de módulos no monólito e seus enfoques:)

[//]: # (| Método de Identificação      | Enfoque e Critérios                                                                                                                                                                                                          | Referências e Exemplos                                                                                                                                                                                                                                                                                                        |)

[//]: # (|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|)

[//]: # (| Análise de Domínio           | Delimitação por subdomínios de negócio &#40;Bounded Contexts&#41;; utiliza artefatos de requisitos &#40;modelos, casos de uso, processos&#41; para identificar temas funcionais.                                                       | DDD, Event Storming, mapas de contexto; Ex.: módulos Vendas, Estoque, Pagamentos. Abgaz et al.: identifica “domínios de interesse” via modelos de negócio.                                                                                                                         |)

[//]: # (| Análise de Responsabilidades | Delimitação por funcionalidades e responsabilidades do código; agrupa elementos por feature ou caso de uso, buscando responsabilidade única por módulo.                                                                 | Revisão de casos de uso e mapeamento para componentes de código; princípio SRP &#40;Single Responsibility Principle&#41; aplicado a módulos. Ex.: Usuários vs Relatórios.                                                                                                               |)

[//]: # (| Análise Estática &#40;Código&#41;     | Estudo de dependências estruturais no código &#40;chamadas, imports, acesso a dados&#41;; identificação de clusters com alta coesão interna e baixo acoplamento externo.                                                        | Grafos de chamadas, DSM &#40;matriz de estrutura&#41;; algoritmos de clustering &#40;hierárquico, k-means, comunidade&#41;. Ferramentas: jDepend, Arcan, Service Cutter.                                                                                                                         |)

[//]: # (| Análise Dinâmica &#40;Execução&#41;  | Observação de interações em tempo de execução; agrupa componentes que colaboram nos mesmos fluxos de uso e analisa frequência de interações.                                                                                  | Tracing/log de chamadas, profiling de módulos ativos por caso de uso. Ferramentas: Kieker &#40;instrumentação Java&#41;, logs de acesso e APM. Ex.: monitoração de chamada entre serviços internos durante operações críticas.                                                             |)

[//]: # (| Análise de Versões           | Mineração do histórico de código; detecção de arquivos que mudam juntos frequentemente &#40;acoplamento evolutivo&#41; e padrões de co-mudança indicando dependências lógicas.                                                 | Métricas de co-change, análise de commits Git. Ex.: scripts para contar commits comuns entre módulos; uso de heurísticas de evolução para pesar grafos estáticos. Ferramenta experimental: Microservice Miner integrando dados evolutivos.                                        |)

[//]: # (| Feedback de Stakeholders     | Consulta a desenvolvedores, arquitetos e usuários-chave para validar e refinar fronteiras; considera conhecimento não explícito no código &#40;ex.: prioridades de negócio, restrições organizacionais&#41;.                   | Workshops, entrevistas, brainstorming com domain experts. Ex.: equipe confirma que Faturamento deve ser separado de Pedidos por requisitos legais, mesmo que haja interdependência técnica &#40;justificando refatoração&#41;.                                                              |)

### Tabela 1: Principais Métodos de Identificação de Módulos no Monólito

| Método de Identificação      | Descrição                                                                                                                                                                                                          | Exemplos/Indicadores                                                                                                                                                                                                                                                                                                        |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Análise de Domínio**       | Delimitação por subdomínios de negócio (*Bounded Contexts*); utiliza artefatos de requisitos (modelos, casos de uso, processos) para identificar temas funcionais.                                         | - Técnicas: DDD, Event Storming, mapeamento de contextos;<br>- Ex.: módulos Vendas, Estoque, Pagamentos;<br>- Abgaz et al.: identificação de “domínios de interesse” a partir dos modelos de negócio.                                                                                                                     |
| **Análise de Responsabilidades** | Delimitação por funcionalidades e responsabilidades do código; agrupa elementos por feature ou caso de uso, buscando a responsabilidade única de cada módulo.                                        | - Foco em revisão de casos de uso e mapeamento para componentes de código;<br>- Princípio SRP (Single Responsibility Principle) aplicado aos módulos;<br>- Ex.: separar módulo de Usuários do módulo de Relatórios.                                                                                                    |
| **Análise Estática (Código)**     | Estudo de dependências estruturais no código (chamadas, imports, acesso a dados); identificação de clusters com alta coesão interna e baixo acoplamento externo.                                     | - Uso de grafos de chamadas e DSM (matriz de estrutura);<br>- Algoritmos de clustering (hierárquico, k-means, detecção de comunidades);<br>- Ferramentas: jDepend, Arcan, Service Cutter.                                                                                                                              |
| **Análise Dinâmica (Execução)**  | Observação de interações em tempo de execução; agrupa componentes que colaboram nos mesmos fluxos de uso e analisa a frequência de interações.                                                       | - Utilização de tracing, logs e profiling para identificar fluxos de uso;<br>- Ferramentas: Kieker (instrumentação Java), logs de acesso e APM;<br>- Ex.: monitoramento de chamadas entre serviços internos durante operações críticas.                                                                          |
| **Análise de Versões**           | Mineração do histórico de código; detecta arquivos que mudam juntos frequentemente (acoplamento evolutivo) e padrões de co-mudança que indicam dependências lógicas.                              | - Uso de métricas de co-change e análise de commits Git;<br>- Ex.: scripts para contar commits comuns entre módulos;<br>- Ferramenta experimental: Microservice Miner que integra dados evolutivos para pesar grafos estáticos.                                                                                       |
| **Feedback de Stakeholders**     | Consulta a desenvolvedores, arquitetos e usuários-chave para validar e refinar fronteiras; considera conhecimentos não explícitos no código (prioridades de negócio, restrições organizacionais). | - Realização de workshops, entrevistas e sessões de brainstorming com domain experts;<br>- Ex.: confirmação de que o módulo Faturamento deve ser separado do módulo Pedidos devido a exigências legais, mesmo com interdependência técnica.                                                                  |
Tabela 1: Métodos para identificação de módulos em um monólito e critérios utilizados.

Em resumo, identificar módulos em um monólito é um processo multidimensional. Métodos baseados em domínio garantem alinhamento com as fronteiras naturais do negócio; análises do código (estática e dinâmica) fornecem evidências técnicas de dependências e interações; a análise de versões revela laços históricos de manutenção; e o feedback humano assegura que os módulos farão sentido na prática. A literatura sobre migração para microsserviços oferece um arcabouço rico de técnicas que pode (e deve) ser aproveitado também na modularização interna de um monólito ￼ – afinal, um monólito modular bem delineado muitas vezes difere de um conjunto de microsserviços apenas na forma de implantação e não nos princípios de separação por trás dos módulos.

## 4. Como avaliar a Qualidade dos Módulos Identificados (“Módulos Bons”)

Após identificar candidatos a módulo, é essencial avaliar se essas divisões são adequadas – isto é, se atendem a critérios de qualidade de arquitetura modular. Módulos “bons” devem exibir baixo acoplamento entre si e alta coesão interna, além de facilitar testabilidade, manutenção, evolução e eventualmente suportar escalabilidade da aplicação. A avaliação pode ser realizada tanto por métricas objetivas quanto por análise qualitativa (inspeções e feedback da equipe). Abaixo, discutimos os principais critérios de qualidade e como saber se os módulos atendem a cada um, bem como práticas de refinamento contínuo.

### Baixo Acoplamento

Acoplamento se refere ao grau de dependência entre módulos – módulos bem projetados minimizam essas dependências. Na prática, isso significa que um módulo deve poder funcionar (e ser desenvolvido ou modificado) com pouca ou nenhuma necessidade de conhecer detalhes internos de outros módulos. Critérios de baixo acoplamento incluem:
-	Interfaces Claras e Poucas Dependências Externas: Cada módulo idealmente expõe uma interface (API interna) e oculta seus detalhes. O número de chamadas de um módulo A para outro B deve ser pequeno e bem definido. Métricas de efferent coupling (número de outros módulos que um módulo invoca) e afferent coupling (número de módulos que invocam este módulo) quantificam isso ￼ ￼. Por exemplo, se o módulo Relatórios depende de 5 outros módulos para funcionar, ele tem alto acoplamento de saída (efferent); isso pode indicar que sua função não está bem delimitada ou que deveria ser subdividido.
-	Independência de Implementação: Mudanças internas em um módulo não deveriam quebrar outros módulos. Um sinal de baixo acoplamento é quando modificações no código de um módulo raramente requerem alterações em outro – algo que pode ser medido pelo histórico de commits (poucas co-mudanças entre módulos) e também observado via testes de regressão (módulo A pode ser alterado sem falhar testes do módulo B).
-	Métricas de Acoplamento: Estudos acadêmicos apontam coupling e cohesion como as métricas mais tradicionais e importantes na avaliação de modularidade ￼. Variantes objetivas incluem: instabilidade (proporção de dependências de saída sobre total de dependências – valor baixo indica módulo mais estável, pois muitos dependem dele mas ele depende de poucos ￼), número de interfaces públicas (por ex., métrica IFN: quantas interfaces o módulo expõe para outros ￼), ou simplesmente contagem de chamadas inter-modulares. Um diagrama de dependência entre módulos deve idealmente formar um gráfico desacoplado (de preferência acíclico) – se houver ciclos de acoplamento (A depende de B e B de A), a modularização está comprometida. Ferramentas como análise de Dependency Structure Matrix (DSM) podem calcular um índice de acoplamento geral do sistema. Em suma, “baixo acoplamento” se manifesta numericamente por poucos pontos de interação entre módulos e baixo fluxo de dependências em ambos os sentidos ￼.

### Alta Coesão

Coesão é o grau em que os elementos internos de um módulo estão relacionados e trabalham juntos para a mesma finalidade. Um módulo altamente coeso concentra funcionalidades estreitamente ligadas entre si (por exemplo, todas referentes a gerenciamento de clientes) e não mistura responsabilidades díspares. Para avaliar coesão:
-	Responsabilidade Única: Cada módulo deve implementar idealmente uma responsabilidade ou funcionalidade central do sistema (ou um conjunto de funcionalidades muito relacionadas). Abgaz et al. apontam que coesão pode ser vista como medida do “princípio da responsabilidade única” aplicado ao serviço/módulo ￼ ￼. Se ao revisar um módulo encontramos múltiplos objetivos não relacionados (ex.: o módulo Usuários também gera relatórios financeiros), então ele tem coesão baixa. Revisões de design e inspeções manuais ajudam a detectar esses cheiros de baixa coesão.
-	Interconexão Interna vs. Externa: Um indicador é a razão entre comunicações internas e externas. Métricas de coesão clássicas como LCOM (Lack of Cohesion of Methods) podem ser agregadas para o nível de módulo – LCOM alto indica que as classes do módulo pouco interagem entre si (baixa coesão) ￼. Outras métricas citadas incluem CHM (Cohesion at Message level) e CHD (Cohesion at Domain level) que quantificam coesão considerando comunicação entre classes e similaridade de conceitos no domínio do módulo ￼. Por exemplo, CHD verifica se as classes de um módulo compartilham muitos termos de domínio (alta similaridade conceitual significa alta coesão temática) ￼. Se um módulo tem funções totalmente independentes que nunca se chamam ou não compartilham conceitos de dados, possivelmente deveria ser separado em módulos menores.
-	Cálculo de Coesão Estrutural: Algumas abordagens medem coesão pela densidade de conexões internas: número de chamadas ou relações entre componentes dentro do módulo em relação ao número máximo possível. Também existe o conceito de modularidade estrutural (SMQ) que combina similaridade estrutural (número de chamadas internas) e similaridade conceitual (vocabulário comum) para avaliar o quão forte é a unidade formada pelo módulo ￼ ￼. Módulos bons tendem a ter SMQ alto – seus componentes são fortemente conectados e semânticamente próximos.
-	Verificação Empírica: Podemos olhar para a implementação: se ao fazer uma mudança funcional, quase todas as alterações ocorrem dentro de um mesmo módulo, isso sugere boa coesão (o módulo concentra a lógica relevante). Por outro lado, se implementar uma única feature exige tocar muitos módulos diferentes, talvez as responsabilidades estejam mal distribuídas (coesão difusa). Durante a fase de prototipação, construir mapas de calor de mudanças ou execução dentro de módulos por funcionalidade pode revelar se cada módulo é internamente coeso em termos de comportamento.

Resumo de Acoplamento vs. Coesão: Em geral, buscamos baixo acoplamento entre módulos e alta coesão dentro de cada módulo – essa combinação favorece a independência e a clareza. Abgaz et al. enfatizam que praticamente todas as extrações automatizadas dependem de variações desses critérios de acoplamento/cohesão ￼. Uma boa modularização normalmente aparece como tal também nas métricas: acoplamento (dependência externa) baixo e coesão (conectividade interna) alta são objetivos quantificáveis ￼ ￼.

### Facilidade de Testes (Testabilidade)

Módulos bem definidos facilitam a escrita de testes isolados para suas funcionalidades, pois possuem interfaces claras e poucas dependências externas. Para avaliar testabilidade:

- Capacidade de Isolamento: Deve ser possível instanciar ou utilizar um módulo (ou seus componentes principais) em um ambiente de teste simulando ou “mockando” interações com outros módulos. Se há baixo acoplamento, consegue-se substituir módulos vizinhos por dublês (mocks/stubs) facilmente nos testes. Por exemplo, um módulo Catálogo pode ser testado simulando-se as respostas do módulo Usuários se precisar de autenticação, mas sem precisar de toda a aplicação rodando.
- Cobertura de Testes por Módulo: Verifica-se se cada módulo pode atingir alta cobertura de testes unitários/integrados focados nele próprio. Se certos módulos são difíceis de testar isoladamente (ex.: porque invocam em demasia serviços globais ou dependem de estado compartilhado), é indício de modularização imperfeita. Módulos bons expõem pontos de entrada bem definidos para suas funcionalidades que podem ser invocados em testes.
- Tempo de Build/Teste: Em arquiteturas modulares, idealmente deve-se conseguir compilar e testar módulos independentemente. Embora num monólito modular completo isso possa não ser totalmente possível (já que é um só deploy), métricas como o tempo de execução de testes de um módulo podem indicar se ele é razoavelmente independente. Se adicionar um teste para o módulo A requer configurar todo o contexto de B, C e D, há problema de acoplamento.
- Relato Qualitativo dos Desenvolvedores: Muitas vezes, a testabilidade é percebida pela equipe – módulos “bons” são aqueles em que os desenvolvedores conseguem escrever testes de unidade facilmente, enquanto módulos ruins levam a testes complexos ou altamente acoplados. Feedback da equipe de QA e dev é útil: por exemplo, “O módulo X consegue ser testado com mocks simples, mas o Y exige um banco de dados inteiro e vários módulos ligados para testar uma única função”.

Em resumo, um módulo adequado melhora a testabilidade porque isola comportamentos. Isso não apenas aumenta a qualidade do software como serve de confirmação de boa separação de preocupações. Alguns projetos adotam inclusive a prática de primeiro escrever testes de contrato para o que seria cada módulo e verificar se as dependências são mínimas (TDD de arquitetura).

### Manutenibilidade e Evolução

Manutenibilidade engloba quão fácil é compreender, modificar e corrigir um módulo, e evolução refere-se à capacidade de acrescentar novas funcionalidades ou mudanças arquiteturais com impacto controlado. Módulos bem projetados contribuem para:
- Localidade de Mudanças: Alterações de requisitos ou correções de bugs devem afetar apenas um ou poucos módulos. Uma modularização de qualidade exibe baixo espalhamento de mudanças – isto pode ser medido observando-se quantos módulos tocam em média por issue corrigido ou feature implementada. Se frequentemente um único change request implica modificar 5 módulos diferentes, as fronteiras podem não estar adequadas. O objetivo é que cada feature ou concern do sistema esteja encapsulado em um módulo principal.
-	Facilidade de Compreensão: Cada módulo forma uma unidade lógica que pode ser compreendida isoladamente. Isso melhora a analizabilidade (subatributo da manutenibilidade segundo ISO 25010): um novo desenvolvedor consegue entender o módulo Pagamento sem precisar ler todo o resto do sistema. Indicadores podem ser subjetivos (feedback da equipe sobre complexidade) e objetivos (tamanho médio de módulo – módulos extremamente grandes podem ser difíceis de entender, enquanto minúsculos demais podem significar fragmentação desnecessária).
-	Proteção contra Regressões: Com módulos independentes e bem testados, mudanças em um módulo devem causar pouco risco de efeitos colaterais. A presença de baixo acoplamento novamente é chave aqui – se um módulo é modificado e outro quebra, havia um acoplamento oculto. Monitorar bugs introduzidos em áreas aparentemente não relacionadas pode sinalizar problemas de limites modulares.
-	Evolvabilidade Arquitetural: Uma arquitetura modular é mais adaptável a mudanças tecnológicas ou de escala. Por exemplo, se decide-se migrar parte da aplicação para um novo framework, pode-se fazer módulo por módulo. Abgaz et al. notam que acoplamento, coesão, modularidade e evolutibilidade estão correlacionados – bons valores de acoplamento/coesia contribuem diretamente para maior capacidade de evoluir ￼. Até métricas de evolvabilidade foram propostas, mas geralmente se concentram em medir os anteriores (complexidade, dependências, etc. que afetam evolução).

Podemos utilizar métricas de código como suporte: tamanho médio do módulo (em linhas de código ou número de classes) – nem tão grande que seja um “mini-monólito”, nem tão pequeno que dispersa lógica coesa; índice de complexidade ciclomática por módulo; razões de comentário ou documentação por módulo (módulos confusos tendem a exigir mais documentação para serem entendidos). No entanto, manutenibilidade tem um forte componente qualitativo. Revisões de código e design periódicas são uma ferramenta: arquitetos podem verificar se as mudanças recentes se confinaram aos módulos esperados ou se estão violando a arquitetura. Se um desenvolvedor frequentemente comenta “precisei mexer em módulo X porque a lógica que eu precisava estava lá mas na verdade era parte da minha história em Y”, isso é um cheiro de modularização incorreta.

Em suma, um módulo “bom” torna mais fácil consertar e crescer o sistema. Já módulos mal delineados geram efeitos cascata de mudança, são difíceis de entender isoladamente e geralmente viram gargalos no desenvolvimento.

### Escalabilidade (de Desenvolvimento e de Operação)

Embora a escalabilidade de desempenho em runtime seja mais relacionada a arquiteturas distribuídas, a modularização influencia escalabilidade em dois sentidos: a escalabilidade do desenvolvimento (múltiplos times trabalhando em paralelo) e a preparação para escalabilidade técnica futura (por exemplo, extrair um módulo para um microsserviço independente para escalar horizontalmente apenas aquela parte).
-	Escalabilidade do Desenvolvimento (Parallelismo): Com módulos claramente separados e de baixo acoplamento, diferentes equipes ou desenvolvedores podem trabalhar em módulos diferentes simultaneamente com mínimo interferência. Ou seja, a arquitetura suporta desenvolvimento paralelo escalável. Se os módulos são bem definidos, cada equipe pode ter propriedade de um conjunto deles (Conway já dizia: a estrutura organizacional reflete-se na modularização do software). Um sinal de boa modularidade aqui é quando merges e conflitos no versionamento diminuem – porque diferentes módulos implicam menos colisão no mesmo arquivo. Organizações muitas vezes avaliam isso atribuindo “áreas de código” a times: se essas áreas raramente se sobrepõem, a modularização ajuda a escalar a equipe.
-	Prontidão para Escalar Arquiteturalmente: Em termos de desempenho e implantação, um monólito modular bem projetado permite que módulos críticos sejam extraídos em microsserviços no futuro com esforço controlado ￼ ￼. Por exemplo, se o módulo Processamento de Imagens começa a demandar escalabilidade específica, ele poderia ser destacado do monólito sem grandes refatorações – isso será viável se suas dependências com o restante já estão limitadas por interfaces claras (baixo acoplamento) e ele já gerencia seus próprios dados ou esquemas. Assim, avaliar escalabilidade é também simular cenários de carga: um módulo intensivo (ex.: Busca) pode tornar-se candidato à separação; se atualmente esse módulo está fortemente acoplado a outros, a modularização não é ótima do ponto de vista de escalabilidade.
-	Métricas de Performance por Módulo: Embora dentro de um monólito puro não se isole facilmente consumo de recursos por módulo, pode-se instrumentar para ver CPU/memória gastos em funcionalidades correspondentes a cada módulo. Módulos independentes permitirão escalabilidade selectiva – em microservices isso seria escalar só aquele serviço. No monólito modular, isso poderia significar permitir threads ou recursos específicos para aquele subsistema. Se um módulo é muito acoplado, não conseguimos isolá-lo para otimizar desempenho. Em avaliações experimentais, pesquisadores mediram tempos de execução ou consumo antes e depois de extrair serviços ￼; aplicando analogamente, se modularização interna não penaliza desempenho e talvez até melhore (por organização de código), é um ponto positivo. De todo modo, escalabilidade no monólito modular é principalmente um atributo de estrutura para futuro – módulos bem separados facilitam dividir a aplicação quando necessário (seja via microsserviços, seja carregando módulos dinamicamente, etc.).

### Uso de Métricas Objetivas

Conforme mencionado, diversas métricas quantitativas auxiliam na avaliação de acoplamento e coesão. Recapitulando as principais e como interpretá-las:
-	Número de Dependências Entre Módulos: contagem de chamadas ou referências de dados de um módulo a outro. Deve ser minimizado. Pode-se montar uma matriz módulo x módulo de dependências e calcular, por exemplo, a porcentagem de células não vazias (um Coupling Factor global). Idealmente poucos pares de módulos se comunicam, e quando o fazem, é através de um número limitado de pontos.
-	Afferent/Efferent Coupling: número de módulos que dependem de X (afferent) versus de quantos X depende (efferent). Módulos centrais e estáveis têm afferent alto e efferent baixo; módulos utilitários ou de nível inferior podem ter efferent um pouco maior. Extremes são ruins: efferent muito alto (módulo depende de muitos outros) = instável; afferent muito alto (módulo muito dependido) mas se não for bem projetado = potencial gargalo para mudanças. Essas métricas vêm de Robert Martin e foram usadas em pesquisas ￼ ￼.
-	Instabilidade (I): calculada como Efferent / (Afferent + Efferent) – varia de 0 (módulo totalmente independente, só dependências entrantes) a 1 (módulo “dependente” de todos, nenhuma dependência entra). Em sistemas bem arquitetados, espera-se módulos de núcleo mais estáveis (I próximo de 0) e módulos periféricos mais instáveis (I mais alto), mas nenhum deveria ser extremo. Avaliar o I médio do sistema ou identificar módulos com I anômalo ajuda a apontar possíveis problemas ￼.
-	Lack of Cohesion of Methods (LCOM) ou variações: originalmente para classes, mas pode ser aplicado agrupando métodos de classes de um módulo. Um LCOM alto sugere que o módulo realiza tarefas diversas não relacionadas. Pesquisas reportam uso de LCOM adaptado para avaliar microserviços ￼ – o mesmo vale para módulos: se métodos dentro do módulo não compartilham estado ou uso comum, o módulo pode carecer de foco.
-	Tamanho do Módulo: número de classes ou LOC. Embora não haja um número “certo”, extremos são indesejáveis. Alguns trabalhos consideram métrica de distribuição de tamanhos – por ex., Non-Extreme Distribution (NED) mede se há equilíbrio no tamanho dos clusters (evitando módulos enormes ou minúsculos) ￼. Um módulo muito grande pode violar coesão (contém submódulos implícitos dentro dele), enquanto módulos muito pequenos talvez aumentem acoplamento (se dividiu funcionalidade coesa em pedaços arbitrários). A modularização deve ser granular o suficiente para separar domínios, mas não tanto a ponto de fragmentar indevidamente funcionalidades fortemente relacionadas.
-	Cobertura de testes por módulo: percentagem de código de cada módulo coberta por testes. Módulos com baixa cobertura podem indicar dificuldades de teste (talvez acoplamento alto impedindo isolamento) ou arquitetura não testável. Esse é um indicador indireto de qualidade modular.
-	Métricas de qualidade internas: como complexidade ciclomática média por classe do módulo, profundidade de dependência interna, etc., podem complementar. Um módulo pode ser coeso mas internamente muito complexo – isso é um problema de design interno, não tanto de modularização, mas se um módulo apresenta complexidade extrema, talvez pudesse ser subdividido em sub-módulos ou componentes internos.

Na literatura de avaliação de decomposição, observou-se que quase todos os trabalhos recentes utilizam métricas para avaliar seus módulos/serviços candidatos ￼ ￼. Coupling e cohesion aparecem consistentemente como os mais importantes e com suporte de longa data ￼, embora com variações de definição em cada estudo. Portanto, na prática industrial, faz sentido adotar um conjunto padrão de métricas para acompanhar a qualidade modular ao longo do tempo ￼. Ferramentas como SonarQube podem ser configuradas para rastrear acoplamento entre componentes, e há bibliotecas para calcular métricas como instability e LCOM.

### Avaliação Qualitativa e Critérios Adicionais

Além das métricas, avaliação qualitativa por arquitetos e desenvolvedores experientes é crucial:
-	Revisões Arquiteturais: Após a modularização, realizar code reviews focadas na arquitetura – verificar se cada módulo tem uma intenção clara, se as interfaces estão bem definidas, se não há “atalhos” entre módulos (e.g., um módulo acessando banco de outro diretamente, o que violaria a independência). Checklist qualitativo: O nome do módulo reflete bem sua responsabilidade? Poderia este módulo ser extraído como um serviço independente facilmente? Há partes do código que ainda parecem pertencer a outro módulo?
-	Feedback de Desenvolvedores: Coletar a opinião da equipe que trabalha com o código modularizado: eles sentem que agora o código está mais organizado? Os módulos correspondem às expectativas? Há ainda áreas de confusão? Muitas vezes, desenvolvedores apontam “este módulo A ainda está muito acoplado com B, sempre que mexemos em A temos que tocar B” – essa informação qualitativa direciona refinamentos. Satisfação do desenvolvedor pode ser um sinal: módulos bons tendem a ser apreciados pela facilidade, módulos ruins geram reclamações.
-	Validação com Stakeholders de Negócio: Embora em menor grau, perguntar a partes interessadas se as fronteiras modulares correspondem a divisões de domínio compreensíveis para elas ajuda a confirmar alinhamento semântico. Por exemplo, a equipe de suporte consegue facilmente delimitar que um bug pertence ao módulo X? Se a modularização faz sentido, termos de negócio usados nos módulos serão familiares aos stakeholders.
-	Conformidade com Requisitos Não-Funcionais: Avaliar se a modularização atende a restrições específicas, p. ex., um módulo pode ser responsável por dados regulados (LGPD, PCI) e assim exigia isolamento – se foi devidamente isolado, atende a esse requisito. Ou requisitos de performance: um módulo crítico foi mantido pequeno e otimizado.

Outros critérios de modularidade incluem reutilização (um módulo bem definido poderia em tese ser reutilizado em outro contexto ou projeto), mas na prática para monólitos modulares o foco principal é facilitar a manutenção incremental do mesmo sistema.

### Refinamento Contínuo dos Módulos (Iteratividade)

A avaliação de modularização não é um evento único – ela deve ser contínua e iterativa. À medida que o sistema evolui, pode ser necessário refinar os limites dos módulos. Novos requisitos podem sugerir subdividir um módulo muito grande ou, inversamente, fundir módulos cuja separação se mostrou artificial. Boas práticas para esse processo contínuo incluem:
-	Medição Contínua de Métricas: Integrar as métricas de acoplamento/coesão no pipeline de integração contínua ou em relatórios periódicos. Assim, a equipe pode monitorar tendências – por exemplo, se o acoplamento entre dois módulos está aumentando ao longo do tempo (talvez devido a novas funcionalidades criando ligações), isso sinaliza necessidade de intervenção arquitetural antes que vire débito técnico severo. Abgaz et al. apontam a falta de padronização de métricas como um gap na literatura ￼, mas recomendam estabelecer um conjunto consistente para uso prático ￼. Em contexto industrial, pode-se adotar um score de modularidade combinando vários indicadores e acompanhar sua variação release a release.
-	Refinamento Iterativo: Adotar uma abordagem ágil para arquitetura, onde a modularização é revista em retrospectivas técnicas. Por exemplo, após cada versão maior, o time de arquitetura revisita o desenho modular: Os módulos ainda fazem sentido com as novas features adicionadas? Algum módulo cresceu demais e deveria ser quebrado? Dois módulos acabaram interagindo mais do que o previsto – precisamos redefinir suas fronteiras? Esse processo incremental garante que a arquitetura não degrade com o tempo.
-	Provas de Conceito e Experimentos: Para módulos críticos ou duvidosos, pode-se fazer pequenos experimentos de refatoração antes de um compromisso maior. Ex: suspeita-se que os módulos Pedido e Carrinho estão mal separados – experimentar fundi-los temporariamente ou separar uma parte como módulo próprio em um branch para ver se métricas e clareza melhoram. Esse tipo de spike arquitetural ajuda a tomar decisões embasadas.
-	Feedback de Usuários e Requisitos Novos: Curiosamente, até feedback de usuários finais pode orientar modularização – se usuários pedem muitas evoluções num certo conjunto de funcionalidades que atualmente está espalhado em vários módulos, isso indica potencial ganho em reorganizá-los. Do ponto de vista de produto, manter módulos alinhados com áreas de valor do usuário facilita planejar releases independentes por módulo (no contexto de microsserviços, isso é evidente; no modular monolith, isso pode significar equipes focadas em módulos).
-	Documentação e Conhecimento Compartilhado: Manter a documentação da arquitetura modular atualizada (por exemplo, o mapa de módulos e suas dependências) é importante para novos membros entenderem e para evitar violações inadvertidas. Quando alguém trabalha sem saber dos limites, pode introduzir um acesso incorreto entre módulos – daí a importância de comunicar claramente a estrutura modular e talvez automatizar enforcement (ferramentas que impedem dependências indevidas, como restrições de pacote ou análises estáticas personalizadas).

Em última instância, sabemos que modularizar um sistema grande é um processo iterativo de convergência. Raramente a primeira divisão é perfeita; por isso, aplicar os critérios e métricas acima repetidamente ao longo do tempo, e ajustar conforme necessário, faz parte das boas práticas de engenharia de software. Esse refinamento contínuo garante que o monólito modular permaneça saudável, com módulos verdadeiramente independentes e coesos mesmo com a evolução do sistema. Como sintetizado por Abgaz et al. (2023), mesmo ao optar por um modular monolith em vez de microsserviços, deve-se seguir fases similares de análise e avaliação, e os benefícios serão colhidos em termos de manutenibilidade e qualidade arquitetural geral ￼.

### Tabela 2: Critérios e Métodos para Avaliação da Qualidade dos Módulos

| Critério                     | Descrição                                                                                                                                                                                                                                                                                   | Exemplos/Indicadores                                                                                                                                                                                                                                                                                                                                       |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Baixo Acoplamento**        | Os módulos devem ter poucas dependências entre si, de modo que mudanças em um módulo não exijam alterações significativas em outros.                                                                                                                                                      | - Interfaces claras e restritas (API interna bem definida).<br>- Métricas: *Efferent/Afferent Coupling* e *Instabilidade (I)*.<br>- Diagrama de dependências desacoplado (idealmente acíclico; sem ciclos como A depende de B e B de A).                                                                                                                 |
| **Alta Coesão**              | Os elementos internos de cada módulo devem estar intimamente relacionados, realizando uma única função ou responsabilidade.                                                                                                                                                               | - Responsabilidade única (ex.: um módulo "Usuários" que não engloba funções financeiras).<br>- Métricas: *LCOM* (Lack of Cohesion of Methods), *Cohesion at Message/Domain Level (CHM/CHD)* ou *SMQ* (modularidade estrutural).<br>- Relação entre comunicação interna (alta) versus externa (baixa).                                                            |
| **Facilidade de Testes**     | O módulo deve permitir a criação de testes unitários e de integração de forma isolada, evidenciando que sua interface e dependências são bem definidas.                                                                                                                                    | - Capacidade de isolar o módulo por meio de *mocks* ou *stubs*.<br>- Alta cobertura de testes por módulo.<br>- Tempo reduzido de build/test.<br>- Feedback positivo dos desenvolvedores quanto à testabilidade.                                                                                                                                         |
| **Manutenibilidade e Evolução** | O módulo deve ser de fácil compreensão, modificação e correção, permitindo que mudanças de requisitos afetem preferencialmente somente o módulo em questão.                                                                                                                            | - Localidade de mudanças: alterações concentradas em um único módulo.<br>- Facilidade de compreensão (analizabilidade).<br>- Baixo espalhamento de alterações (verificado via histórico de commits ou feedback dos times).                                                                                                                           |
| **Escalabilidade**           | Os módulos devem facilitar o trabalho paralelo de equipes e permitir a extração ou isolamento de funcionalidades críticas, caso seja necessário escalá-las (por exemplo, convertendo um módulo em microsserviço).                                                                    | - Menores conflitos de merge no versionamento.<br>- Separação clara de áreas de código, possibilitando escalabilidade seletiva.<br>- Capacidade de isolar módulos críticos para futura extração quando a demanda por escalabilidade aumentar.                                                                                                               |
| **Métricas Objetivas**       | A utilização de métricas quantitativas auxilia na verificação dos critérios arquiteturais e serve de base para decisões de refinamento e evolução da modularização.                                                                                                                     | - Número de dependências entre módulos (matriz módulo x módulo).<br>- Índice de instabilidade.<br>- LCOM e variações para medir coesão.<br>- Tamanho dos módulos (LOC, número de classes).<br>- Cobertura de testes por módulo.<br>- Ferramentas: SonarQube, Dependency Structure Matrix (DSM) e outras que possibilitem quantificação das dependências. |
| **Avaliação Qualitativa**    | A análise por arquitetos e desenvolvedores é fundamental para confirmar a clareza, o propósito e a viabilidade dos módulos na prática, complementando as métricas quantitativas.                                                                                                  | - Code reviews focadas na arquitetura.<br>- Feedback dos desenvolvedores sobre a facilidade de uso e manutenção dos módulos.<br>- Verificação se os nomes e limites dos módulos refletem o domínio do negócio.<br>- Revisões periódicas para identificar “cheiros” de arquitetura inadequada.                                                          |
| **Refinamento Iterativo**    | A avaliação e a modularização devem ser monitoradas e ajustadas continuamente, acompanhando a evolução do sistema e as necessidades do negócio.                                                                                                                                           | - Integração das métricas no pipeline de CI.<br>- Revisões arquiteturais periódicas (retrospectivas técnicas).<br>- Provas de conceito (spikes) para testar refatorações em módulos suspeitos.<br>- Documentação atualizada do mapa de módulos e dependências.                                                                                           |

## 5. Tecnologias Utilizadas

- **Arquitetura Limpa (AL)**
- **Domain-Driven Design (DDD)**
- **Command Query Responsibility Segregation (CQRS)**
- **Monólito Modular (MOM)**
- **Modelo C4** para documentação arquitetural
- **Docker** para containerização
- **Docker Compose** para orquestração de contêineres
- **CI/CD** (ex.: Azure Pipelines, GitHub Actions) para automação de deploys
- **Bancos de Dados Relacionais e NoSQL**

## 6. Glossário

| Sigla | Descrição                                |
| ----- | ---------------------------------------- |
| AH    | Arquitetura Hexagonal                    |
| AL    | Arquitetura Limpa                        |
| C4    | Modelo C4                                |
| CA    | Clean Architecture                       |
| COD   | Código                                   |
| COM   | Componente                               |
| CON   | Container                                |
| CONT  | Contexto                                 |
| CQRS  | Command Query Responsibility Segregation |
| DDD   | Domain-Driven Design                     |
| DTO   | Data Transfer Object                     |
| EC    | E-commerce                               |
| IDE   | Integrated Development Environment       |
| MI    | Microsserviço                            |
| MO    | Monolítico                               |
| MOM   | Monolito Modular                         |
| PD    | Portas e Adaptadores                     |
| RSL   | Revisão Sistemática da Literatura        |
| S360  | Sales 360                                |
| SIG   | Sistema Integrado de Gestão              |
| UML   | Unified Modeling Language                |

## 7. Referências Utilizadas:
-	Y. Abgaz et al. (2023). “Decomposition of Monolith Applications Into Microservices Architectures: A Systematic Review.” IEEE TSE, 49(8). – (Referência principal para técnicas M2MDF de migração monólito-microsserviços, incluindo análise de domínio, estática, dinâmica, versões e métricas de avaliação).
-	[Demais referências acadêmicas e técnicas foram citadas inline ao longo do texto conforme o formato requerido, evidenciando métodos (e.g. Service Cutter ￼, algoritmos de clustering ￼) e métricas (e.g. acoplamento/cohesão ￼ ￼) pertinentes.]


[//]: # (## 4. Como Saber se os Módulos Estão Bons?)

[//]: # (Após a identificação dos módulos, é fundamental avaliar a qualidade da divisão adotada. Alguns indicadores de uma boa modularização são:)

[//]: # ()
[//]: # (- **Baixo Acoplamento:**)

[//]: # (    - Os módulos devem ter poucas dependências entre si, comunicando-se apenas por meio de interfaces bem definidas ou eventos. Isso facilita a manutenção e a substituição de componentes sem impacto significativo no sistema.)

[//]: # ()
[//]: # (- **Alta Coesão:**)

[//]: # (    - Cada módulo deve agrupar funcionalidades que fazem parte do mesmo domínio de negócio, garantindo que todas as partes do módulo trabalham de forma integrada.)

[//]: # ()
[//]: # (- **Facilidade de Testes:**)

[//]: # (    - Módulos bem definidos permitem a criação de testes unitários e de integração mais eficazes, auxiliando na identificação e correção de problemas.)

[//]: # ()
[//]: # (- **Manutenção e Evolução:**)

[//]: # (    - A modularização deve facilitar a implementação de mudanças. Se alterações em um módulo não afetarem outros de maneira inesperada, isso indica uma boa separação de responsabilidades.)

[//]: # ()
[//]: # (- **Performance e Escalabilidade:**)

[//]: # (    - A possibilidade de escalar de forma independente os módulos que demandam maior processamento é um indicativo de uma boa divisão.)

[//]: # ()
[//]: # (- **Feedback Contínuo e Métricas:**)

[//]: # (    - Utilize métricas de performance, logs e feedback dos usuários para ajustar a estrutura modular ao longo do tempo, garantindo que ela atenda às necessidades do negócio.## 4. Como Saber se os Módulos Estão Bons?)

[//]: # ()
[//]: # (Após a identificação dos módulos, é fundamental avaliar a qualidade da divisão adotada. Alguns indicadores de uma boa modularização são:)

[//]: # ()
[//]: # (- **Baixo Acoplamento:**)

[//]: # (    - Os módulos devem ter poucas dependências entre si, comunicando-se apenas por meio de interfaces bem definidas ou eventos. Isso facilita a manutenção e a substituição de componentes sem impacto significativo no sistema.)

[//]: # (- **Alta Coesão:**)

[//]: # (    - Cada módulo deve agrupar funcionalidades que fazem parte do mesmo domínio de negócio, garantindo que todas as partes do módulo trabalham de forma integrada.)

[//]: # ()
[//]: # (- **Facilidade de Testes:**)

[//]: # (    - Módulos bem definidos permitem a criação de testes unitários e de integração mais eficazes, auxiliando na identificação e correção de problemas.)

[//]: # ()
[//]: # (- **Manutenção e Evolução:**)

[//]: # (    - A modularização deve facilitar a implementação de mudanças. Se alterações em um módulo não afetarem outros de maneira inesperada, isso indica uma boa separação de responsabilidades.)

[//]: # ()
[//]: # (- **Performance e Escalabilidade:**)

[//]: # (    - A possibilidade de escalar de forma independente os módulos que demandam maior processamento é um indicativo de uma boa divisão.)

[//]: # ()
[//]: # (- **Feedback Contínuo e Métricas:**)

[//]: # (    - Utilize métricas de performance, logs e feedback dos usuários para ajustar a estrutura modular ao longo do tempo, garantindo que ela atenda às necessidades do negócio.)