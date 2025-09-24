# Certificação Laravel Senior

## Tópicos

### Arquitetura e Design Patterns (Padrões de Projeto)
#### Introdução
Este é o tópico mais crucial para um desenvolvedor sênior. Não basta saber como usar o Laravel, mas por que ele é estruturado de tal forma. O domínio de padrões de arquitetura e design garante que você possa construir aplicações complexas, escaláveis e de fácil manutenção.
#### Assuntos
- Service Container (Injeção de Dependência);
- Service Providers;
- Facades;
- Contratos (Contracts);
- Padrões como Repository;
- Service, Action e a diferença entre eles;
- Entendimento de SOLID principles no contexto do Laravel.

### Testes (Testing)
#### Introdução
A capacidade de escrever testes automatizados é uma marca registrada de um desenvolvedor sênior. Testes garantem a confiabilidade do código, facilitam refatorações e previnem regressões. Em um ambiente profissional, código sem testes é considerado incompleto.
#### Assuntos
- Testes unitários, de integração e de funcionalidades (feature tests) com Pest e PHPUnit;
- Test-Driven Development (TDD);
- Mocking de objetos, fakes, stubs e o uso de factories para popular dados de teste;
- Testes de API e de interações com o banco de dados.

### Filas, Jobs e Tarefas em Background (Queues & Jobs)
#### Introdução
Para qualquer aplicação que precise executar tarefas demoradas (como enviar e-mails, processar imagens ou gerar relatórios) sem bloquear a interface do usuário, o sistema de filas é essencial. Um sênior deve saber como e quando utilizá-lo para melhorar a performance e a experiência do usuário.
#### Assuntos
- Configuração de drivers de fila (Redis, SQS, Database);
- Criação de Jobs;
- Despacho de Jobs síncronos e assíncronos;
- Tratamento de falhas (failed jobs);
- Horizon para monitoramento de filas e otimização de workers.

### Performance e Otimização
#### Introdução
Um desenvolvedor sênior é responsável por garantir que a aplicação seja rápida e eficiente. Isso envolve identificar gargalos de performance, otimizar queries de banco de dados e utilizar as ferramentas de cache do Laravel de forma inteligente.
#### Assuntos
- Eager Loading (N+1 query problem);
- Caching de queries;
- Cache de rotas e configurações;
- Uso de Laravel Octane para servidores de aplicação de alta performance;
- Otimização de assets com Vite/Mix;
- Profiling de código com ferramentas como Laravel Telescope ou Blackfire.

### Segurança (Security)
#### Introdução
A segurança não é opcional. Desenvolvedores sênior devem ter uma mentalidade proativa em relação à proteção da aplicação contra vulnerabilidades comuns da web.
#### Assuntos
- Proteção contra Cross-Site Scripting (XSS);
- Cross-Site Request Forgery (CSRF);
- SQL Injection;
- Autenticação e Autorização (Gates e Policies);
- Mass Assignment;
- Validação de dados de entrada e sanitização de saída.

### Banco de Dados e Eloquent Avançado
#### Introdução
Embora o básico do Eloquent seja simples, um sênior deve dominar seus recursos avançados para manipular dados de forma eficiente e elegante.
#### Assuntos
- Relacionamentos polimórficos;
- Relacionamentos many-to-many com campos extras (pivot tables);
- Query scopes (locais e globais);
- Model observers;
- Collections avançadas;
- Transações de banco de dados e locks (pessimistic/optimistic).

### Desenvolvimento de APIs RESTful e Autenticação
#### Introdução
Em um mundo de microserviços e front-ends desacoplados (SPAs, mobile), a criação de APIs robustas é uma habilidade fundamental.
#### Assuntos
- Desenvolvimento de rotas de API;
- Resources para transformação de dados (JSON);
- Tratamento de erros e códigos de status HTTP;
- Autenticação de APIs via Laravel Sanctum (para SPAs e tokens de API) e Passport (para OAuth2).

### Ecossistema Laravel
#### Introdução
Conhecer as ferramentas oficiais que orbitam o framework demonstra um comprometimento com a plataforma e a capacidade de construir soluções completas e modernas.
#### Assuntos
- Laravel Horizon (monitoramento de filas);
- Telescope (debugging);
- Sanctum (autenticação);
- Breeze/Jetstream (scaffolding de autenticação);
- Scout (busca full-text);
- Cashier (gerenciamento de assinaturas).

### Desenvolvimento de Pacotes (Package Development)
#### Introdução
A capacidade de extrair funcionalidades reutilizáveis para pacotes privados ou de código aberto é um grande diferencial. Isso demonstra um entendimento profundo de como o Laravel funciona internamente e promove a reutilização de código.
#### Assuntos
- Estrutura de um pacote;
- Uso de Service Providers para registrar rotas, views e configurações;
- Publicação de assets.

### Conceitos de Frontend e Integração
#### Introdução
Embora não seja um especialista em frontend, um desenvolvedor sênior de Laravel deve entender como o backend se integra com o frontend moderno.
#### Assuntos
- Integração com frameworks JavaScript (Vue.js, React) usando Inertia.js;
- Compilação de assets com Vite;
- Autenticação para Single Page Applications (SPAs).
