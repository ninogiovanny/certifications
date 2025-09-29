### Senior Laravel Certification Questions: Facades

#### Questão 1
Qual método abstrato obrigatório deve ser implementado ao criar uma classe de Facade personalizada que estende Illuminate\Support\Facades\Facade?

- A) `getFacadeRoot()`
- B) `resolveFacadeInstance()`
- C) `getFacadeAccessor()`
- D) `bindInstance()`

Explicação:

Ao criar uma Facade personalizada no Laravel, você deve estender a classe base Illuminate\Support\Facades\Facade. Esta classe base exige a implementação de um único método: getFacadeAccessor(). O valor retornado por este método é a "chave" (geralmente o nome da classe ou uma string única) que o Laravel usará para resolver a instância do objeto correspondente a partir do contêiner de serviço (Service Container). É através deste método que a "mágica" da Facade acontece, conectando a chamada estática à instância real do objeto.

Resposta Correta: C) getFacadeAccessor()

#### Questão 2
Considere o seguinte código em um Service Provider:

PHP

$this->app->bind('my-custom-class', function ($app) {
    return new \App\Services\MyCustomClass();
});
Como o método getFacadeAccessor() de uma Facade personalizada (MyCustomClassFacade) deveria ser implementado para resolver esta classe?

- A) `protected static function getFacadeAccessor() { return \App\Services\MyCustomClass::class; }`
- B) `protected static function getFacadeAccessor() { return 'my-custom-class'; }`
- C) `protected static function getFacadeAccessor() { return new \App\Services\MyCustomClass(); }`
- D) `protected static function getFacadeAccessor() { return resolve('MyCustomClass'); }`

Explicação:

O método getFacadeAccessor() deve retornar a chave de binding registrada no Service Container. No código fornecido, a classe \App\Services\MyCustomClass foi registrada com a chave de string 'my-custom-class'. Portanto, para que a Facade resolva corretamente a instância, o getFacadeAccessor() deve retornar exatamente essa string. As outras opções estão incorretas porque a opção A retornaria o nome completo da classe (que funcionaria se o binding fosse feito com o nome da classe), a C tenta instanciar a classe diretamente (o que vai contra o propósito da Facade), e a D usa a função resolve de forma incorreta.

Resposta Correta: B) protected static function getFacadeAccessor() { return 'my-custom-class'; }

#### Questão 3
O que o Laravel faz internamente na primeira vez que você chama um método estático em uma Facade, como Cache::get('key')?

- A) Ele cria uma nova instância da classe Facade e chama o método nela.
- B) Ele usa o método mágico `__callStatic()` para resolver a instância do objeto no Service Container e, em seguida, chama o método desejado nessa instância.
- C) Ele reflete a classe Facade para encontrar um método estático correspondente e o executa.
- D) Ele procura por uma função helper global com o mesmo nome e a executa.

Explicação:

As Facades do Laravel fornecem uma interface "estática" para classes que estão disponíveis no contêiner de serviço. Elas não possuem os métodos estáticos que você chama. Em vez disso, o PHP aciona o método mágico __callStatic() na classe Facade base. Este método resolve a instância do objeto do contêiner de serviço (usando o getFacadeAccessor()), e então invoca o método solicitado (get('key') neste caso) na instância do objeto resolvido, passando quaisquer argumentos que foram fornecidos.

Resposta Correta: B) Ele usa o método mágico __callStatic() para resolver a instância do objeto no Service Container e, em seguida, chama o método desejado nessa instância.

#### Questão 4
Qual é a principal vantagem de usar Facades em vez de injetar dependências diretamente no construtor de uma classe?

- A) Facades oferecem um desempenho significativamente melhor.
- B) Facades proporcionam uma sintaxe mais concisa e expressiva, especialmente em contextos que não suportam injeção de dependência facilmente (como routes/web.php).
- C) Facades são a única maneira de acessar serviços do core do Laravel.
- D) Facades evitam o acoplamento com o Service Container.

Explicação:

A principal vantagem das Facades é a conveniência e a sintaxe expressiva. Embora a injeção de dependência seja uma prática excelente e geralmente preferida para manter o baixo acoplamento, em alguns lugares (como arquivos de rota, tinker, ou mesmo dentro de métodos onde a injeção no construtor não é viável), usar uma Facade como Route::get() ou Cache::remember() é muito mais limpo e legível do que resolver manually a instância do contêiner. A alegação de desempenho (A) é falsa; o overhead é mínimo e desprezível. A (C) é falsa; você pode injetar ou resolver qualquer serviço do core. A (D) é o oposto do verdadeiro; Facades são fortemente acopladas ao Service Container.

Resposta Correta: B) Facades proporcionam uma sintaxe mais concisa e expressiva, especialmente em contextos que não suportam injeção de dependência facilmente (como routes/web.php).

#### Questão 5
O que são "Real-time Facades" no Laravel?

- A) São Facades que são compiladas em tempo de execução para melhor desempenho.
- B) São Facades que podem ser usadas para classes que não foram previamente registradas no Service Container, prefixando a classe com Facades\.
- C) São Facades que atualizam seus dados em tempo real usando WebSockets.
- D) São Facades que só podem ser usadas dentro de testes para simular interações em tempo real.

Explicação:

As "Real-time Facades", introduzidas no Laravel, permitem que você trate qualquer classe em sua aplicação como uma Facade, sem precisar criar uma classe de Facade para ela. Você faz isso importando a classe e prefixando-a com Facades. Por exemplo, se você tem uma classe \App\Services\PaymentGateway, você pode usá-la como uma facade em um controlador simplesmente fazendo use Facades\App\Services\PaymentGateway; e depois chamando métodos estaticamente, como PaymentGateway::charge(100);. O Laravel cuidará de resolver a instância de \App\Services\PaymentGateway do contêiner automaticamente.

Resposta Correta: B) São Facades que podem ser usadas para classes que não foram previamente registradas no Service Container, prefixando a classe com Facades\.

#### Questão 6
Ao testar um código que usa a Facade Mail, qual é a maneira recomendada de evitar o envio de e-mails reais e, em vez disso, fazer asserções sobre os e-mails que teriam sido enviados?

- A) Usar `Mail::shouldReceive('send')->once();`
- B) Usar `Mail::fake();`
- C) Configurar o driver de e-mail como log no arquivo .env.testing.
- D) Criar um mock da classe Mailer usando Mockery.

Explicação:

O Laravel fornece um método fake() muito conveniente em várias de suas Facades (Mail, Notification, Queue, Storage, Event, Bus, Http). Chamar Mail::fake() no início do seu teste substitui a implementação real do Mail por uma implementação "falsa" que captura os e-mails em vez de enviá-los. Isso permite que você use métodos de asserção como Mail::assertSent() ou Mail::assertQueued() para verificar se o seu código se comportou como esperado, sem realizar I/O de rede real. Embora as outras opções possam funcionar, fake() é a abordagem mais idiomática, limpa e poderosa oferecida pelo framework.

Resposta Correta: B) Usar Mail::fake();

#### Questão 7
Qual das seguintes afirmações sobre Facades e testes é a mais precisa?

- A) Código que usa Facades é impossível de testar.
- B) Facades podem ser facilmente "mockadas" usando métodos como shouldReceive(), permitindo isolar o código sob teste.
- C) Você deve sempre usar injeção de dependência em vez de Facades para tornar o código testável.
- D) Testar com Facades requer a configuração de um banco de dados de teste completo.

Explicação:

Uma das características mais poderosas das Facades é a sua testabilidade. Como as Facades são resolvidas a partir do Service Container, o Laravel permite que você troque facilmente a instância real por um objeto mock (um "dublê de teste"). A sintaxe Cache::shouldReceive('get')->with('key')->andReturn('value'); é um exemplo claro disso. Ela instrui a Facade Cache a esperar uma chamada ao método get com o argumento 'key' e a retornar 'value' quando isso acontecer. Isso isola seu teste da implementação real do cache, tornando seus testes mais rápidos e confiáveis. Portanto, a afirmação de que Facades são difíceis de testar é um equívoco comum.

Resposta Correta: B) Facades podem ser facilmente "mockadas" usando métodos como shouldReceive(), permitindo isolar o código sob teste.

#### Questão 8
Qual é a principal diferença entre uma Facade e uma função helper global (como cache() ou view())?

- A) Não há diferença; helpers são apenas aliases para Facades.
- B) Facades são mais performáticas que helpers.
- C) Facades oferecem uma sintaxe estática e orientada a objetos, enquanto helpers são chamadas de função procedural. A testabilidade também é um diferencial chave, com Facades tendo métodos de mock integrados como shouldReceive.
- D) Helpers podem ser usados em qualquer lugar, enquanto Facades só podem ser usadas em Controladores.

Explicação:

Embora ambos possam atingir objetivos semelhantes (acessar serviços do Laravel), eles representam paradigmas diferentes. As Facades (Cache::get()) oferecem uma interface estática que se parece com uma API de classe, o que pode ser mais legível para desenvolvedores com background em linguagens estáticas. A principal vantagem das Facades é sua integração com o Mockery para testes, permitindo Cache::shouldReceive(...). Os helpers (cache()) são funções globais que oferecem uma sintaxe mais curta e procedural. Embora você possa obter a instância e "mocká-la" ao usar helpers, a sintaxe das Facades é projetada especificamente para isso, tornando os testes mais limpos.

Resposta Correta: C) Facades oferecem uma sintaxe estática e orientada a objetos, enquanto helpers são chamadas de função procedural. A testabilidade também é um diferencial chave, com Facades tendo métodos de mock integrados como shouldReceive.

#### Questão 9
Se você executar o método DB::getFacadeRoot(), o que ele retornará?

- A) O nome da classe `Illuminate\Database\DatabaseManager`.
- B) Uma instância da classe Illuminate\Database\DatabaseManager.
- C) Uma instância da conexão de banco de dados padrão.
- D) A string 'db'.

Explicação:

O método getFacadeRoot() é um método real na classe Facade base que resolve a instância do objeto do contêiner e a retorna. Ele faz o trabalho que getFacadeAccessor() configura. Portanto, ao chamar DB::getFacadeRoot(), você está pedindo à Facade DB para resolver sua instância subjacente do Service Container. O acessor para a Facade DB é 'db', que por padrão resolve para uma instância de Illuminate\Database\DatabaseManager. Este objeto gerenciador, por sua vez, gerencia todas as conexões de banco de dados.

Resposta Correta: B) Uma instância da classe Illuminate\Database\DatabaseManager.

#### Questão 10
Em qual cenário o uso de uma Facade pode ser considerado um "code smell" ou má prática?

- A) Em um arquivo de rotas (routes/web.php) para definir um endpoint.
- B) Em um teste unitário para mockar uma dependência externa.
- C) Injetando a Facade em um construtor de classe, como public function __construct(Auth $auth).
- D) Usando a Facade Log extensivamente dentro de uma classe de serviço que possui muitas outras dependências e lógica de negócios complexa.

Explicação:

Facades brilham pela conveniência em locais onde a injeção de dependência é difícil. No entanto, dentro de uma classe de serviço complexa, depender de múltiplas Facades pode esconder as verdadeiras dependências da classe. Se uma classe usa Cache::get(), Log::info(), e Queue::dispatch(), suas dependências (CacheManager, LoggerInterface, Dispatcher) não são explícitas em sua assinatura (o construtor). Isso torna mais difícil entender o que a classe precisa para funcionar e pode violar o Princípio de Inversão de Dependência. Nesses casos, injetar as dependências no construtor é uma prática melhor, pois torna as dependências explícitas e a classe mais fácil de testar e manter. A opção C é um exemplo de type-hinting da classe subjacente, não da facade, o que é uma boa prática.

Resposta Correta: D) Usando a Facade Log extensivamente dentro de uma classe de serviço que possui muitas outras dependências e lógica de negócios complexa.

#### Questão 11
O que acontece se o getFacadeAccessor() de uma Facade retornar o nome de uma classe que não está explicitamente registrada no Service Container?

- A) O Laravel lançará uma BindingResolutionException.
- B) O Laravel tentará resolver a classe usando seu mecanismo de auto-wiring (injeção automática), se a classe tiver dependências resolvíveis.
- C) A chamada da Facade retornará null silenciosamente.
- D) O Laravel criará uma instância da classe Facade em si.

Explicação:

O Service Container do Laravel é poderoso o suficiente para instanciar classes que não foram explicitamente "bindadas", desde que suas próprias dependências possam ser resolvidas pelo contêiner (seja por auto-wiring ou por outros bindings). Este recurso é conhecido como "auto-wiring" ou "zero-configuration resolution". Portanto, se getFacadeAccessor() retornar \App\Services\MyService::class e essa classe não tiver sido registrada, o Laravel tentará fazer new \App\Services\MyService() e injetar quaisquer dependências que seu construtor possa ter. Se as dependências não puderem ser resolvidas, então uma BindingResolutionException será lançada.

Resposta Correta: B) O Laravel tentará resolver a classe usando seu mecanismo de auto-wiring (injeção automática), se a classe tiver dependências resolvíveis.

#### Questão 12
Qual comando Artisan pode ser usado para limpar o cache de Facades "Real-time"?

- A) `php artisan cache:clear`
- B) `php artisan config:clear`
- C) `php artisan view:clear`
- D) `php artisan clear-compiled`

Explicação:

Quando você usa "Real-time Facades", o Laravel gera classes proxy para elas e as armazena no diretório bootstrap/cache/ para otimizar o desempenho em requisições subsequentes. Se você fizer alterações na classe original, pode ser necessário limpar esses proxies em cache. O comando php artisan clear-compiled é responsável por remover esses arquivos compilados, incluindo os proxies de Facades em tempo real, garantindo que a nova versão da sua classe seja usada.

Resposta Correta: D) php artisan clear-compiled

#### Questão 13
Como você pode descobrir qual classe está por trás de uma Facade como Cache?

- A) Lendo os comentários do código-fonte da classe `Illuminate\Support\Facades\Cache`.
- B) Usando `dd(get_class(Cache::getFacadeRoot()));`.
- C) Procurando a chave retornada por `Cache::getFacadeAccessor()` no `config/app.php`.
- D) Todas as opções acima são maneiras válidas.

Explicação:

Todas as três opções são métodos válidos para investigar a classe subjacente de uma Facade.
- A) O DocBlock no topo da classe da Facade (Illuminate\Support\Facades\Cache) geralmente tem uma anotação @see que aponta para a classe real.
- B) Chamar Cache::getFacadeRoot() resolve a instância do objeto do contêiner, e get_class() então retorna o nome da classe desse objeto. Esta é a maneira mais programática e definitiva.
- C) O getFacadeAccessor() lhe dará a chave de binding (neste caso, 'cache'). Você pode então procurar por essa chave nos Service Providers registrados em config/app.php para encontrar onde e como ela é registrada.

Resposta Correta: D) Todas as opções acima são maneiras válidas.

#### Questão 14
A afirmação "Facades são um antipadrão porque violam a Inversão de Dependência e usam métodos estáticos globais" é uma crítica comum. Qual é a melhor contra-argumentação do ponto de vista do Laravel?

- A) A afirmação está correta; Facades devem ser evitadas.
- B) Facades não usam métodos estáticos reais; elas usam o método mágico __callStatic, e a instância subjacente é totalmente substituível e testável, o que está alinhado com os benefícios da Inversão de Dependência.
- C) O desempenho ganho com o uso de Facades supera as desvantagens arquiteturais.
- D) As Facades são necessárias para a funcionalidade do core do framework e não podem ser substituídas.

Explicação:

A principal incompreensão na crítica às Facades é pensar que elas são o mesmo que métodos estáticos tradicionais. Métodos estáticos tradicionais são difíceis de testar e criam um acoplamento rígido. As Facades do Laravel, no entanto, são uma "fachada" para o Service Container. A classe subjacente que faz o trabalho não é estática e é resolvida dinamicamente. Isso significa que você pode trocar a implementação no Service Container (por exemplo, durante os testes) sem alterar o código que a consome. Isso atinge o mesmo objetivo da Inversão de Dependência: o código de alto nível depende de uma abstração (a Facade) e não de uma implementação concreta.

Resposta Correta: B) Facades não usam métodos estáticos reais; elas usam o método mágico __callStatic, e a instância subjacente é totalmente substituível e testável, o que está alinhado com os benefícios da Inversão de Dependência.

#### Questão 15
Considere o teste a seguir:

```
PHP

public function test_something()
{
    Http::fake([
        'github.com/*' => Http::response(['user' => 'taylor'], 200),
    ]);
// código que faz uma chamada para a API do GitHub
Http::assertSent(function ($request) {
    return $request->url() == 'https://github.com/api/user';
});
}
```

O que a chamada `Http::fake()` faz neste contexto?
- A) Ela impede todas as chamadas HTTP de saírem da aplicação e permite que respostas específicas sejam simuladas para URLs correspondentes.
- B) Ela configura um servidor HTTP real, mas local, para responder às requisições.
- C) Ela apenas registra as chamadas HTTP, mas permite que elas continuem para o destino real.
- D) Ela lança uma exceção sempre que uma chamada HTTP é tentada.

Explicação:

A Facade Http do Laravel tem um poderoso sistema de "fake" para testes. Http::fake() intercepta qualquer chamada HTTP feita através do cliente HTTP do Laravel. Em vez de fazer uma requisição de rede real, ele verifica se a URL da requisição corresponde a algum dos padrões fornecidos no array. Se houver uma correspondência (como 'github.com/*'), ele retorna a resposta simulada (Http::response(...)) que você definiu. Isso torna seus testes extremamente rápidos, confiáveis e independentes de serviços externos. A função Http::assertSent() pode então ser usada para verificar se uma requisição para uma URL específica foi de fato tentada.

Resposta Correta: A) Ela impede todas as chamadas HTTP de saírem da aplicação e permite que respostas específicas sejam simuladas para URLs correspondentes.

#### Questão 16
Qual é o propósito do array de aliases no arquivo config/app.php em relação às Facades?

- A) Ele mapeia nomes curtos de Facades (ex: 'Cache') para seus namespaces completos (ex: Illuminate\Support\Facades\Cache).
- B) Ele define quais Facades devem ser carregadas em cada requisição.
- C) Ele substitui a necessidade de implementar o método getFacadeAccessor().
- D) Ele é usado apenas para o modo de depuração e não tem efeito em produção.

Explicação:

O array 'aliases' em config/app.php é um mecanismo de conveniência do autoloader do PHP. Ele permite que você use uma classe sem precisar declarar seu namespace completo com a diretiva use. Por exemplo, ao definir 'Cache' => Illuminate\Support\Facades\Cache::class, você pode simplesmente usar \Cache::get() em qualquer lugar do seu código, e o PHP saberá que você se refere à classe Illuminate\Support\Facades\Cache. Sem esse alias, você teria que importar a classe com use Illuminate\Support\Facades\Cache; em cada arquivo.

Resposta Correta: A) Ele mapeia nomes curtos de Facades (ex: 'Cache') para seus namespaces completos (ex: Illuminate\Support\Facades\Cache).

#### Questão 17
Se você precisar chamar um método em uma classe que normalmente é acessada via Facade, mas você tem uma instância dessa classe em uma variável, como você faria a chamada?

- A) É impossível; você deve sempre usar a interface estática da Facade.
- B) `MyFacade::callMethodOnInstance($instance, 'methodName', $args)`.
- C) Simplesmente chame o método na instância do objeto como faria com qualquer outro objeto PHP: `$instance->methodName($args)`.
- D) `MyFacade::swap($instance)->methodName($args)`.

Explicação:

Uma Facade é apenas um proxy para uma instância de objeto real. Se você já tem a instância (por exemplo, através de injeção de dependência ou resolvendo-a diretamente do contêiner com app('my-service')), você pode e deve interagir com ela como um objeto normal. A Facade é uma forma de acessar essa instância, não a única forma de usá-la. Portanto, a sintaxe padrão de objeto do PHP $instance->methodName() é a correta.

Resposta Correta: C) Simplesmente chame o método na instância do objeto como faria com qualquer outro objeto PHP: $instance->methodName($args).

#### Questão 18
O que o método Facade::swap($mock) faz?

- A) Ele substitui permanentemente a instância no Service Container pela instância $mock.
- B) Ele substitui a instância resolvida pela Facade pela instância $mock apenas para a requisição atual ou ciclo de teste.
- C) Ele cria uma cópia da Facade original e a substitui por $mock.
- D) Ele verifica se a instância da Facade é do mesmo tipo que $mock.

Explicação:

Facade::swap($instance) é um método poderoso para testes. Ele permite que você substitua a instância que uma Facade resolveria do contêiner por uma instância de sua escolha (geralmente um mock). Essa substituição é temporária e fica registrada na própria classe Facade. Quando a Facade é acessada novamente, ela usará a instância "trocada" em vez de ir ao Service Container. Ao final do teste, o Laravel normalmente limpa esses "swaps". Isso é útil para controlar dependências em testes sem precisar usar a sintaxe shouldReceive do Mockery.

Resposta Correta: B) Ele substitui a instância resolvida pela Facade pela instância $mock apenas para a requisição atual ou ciclo de teste.

#### Questão 19
Ao criar uma Facade para um pacote (package) do Laravel, onde você normalmente registraria o binding do serviço no Service Container?

- A) No arquivo `config/app.php` do pacote.
- B) No método `boot()` do Service Provider do pacote.
- C) No método `register()` do Service Provider do pacote.
- D) Diretamente no construtor da classe da Facade.

Explicação:

A convenção do Laravel é usar o método register() de um Service Provider para registrar bindings no contêiner de serviço. Este método é projetado especificamente para isso. O método boot() deve ser usado para lógica que depende de outros serviços já terem sido registrados, como registrar rotas, listeners de eventos ou publicar assets. Registrar um binding no register() garante que ele esteja disponível para que outros providers no método boot() possam utilizá-lo.

Resposta Correta: C) No método register() do Service Provider do pacote.

#### Questão 20
Qual das seguintes opções NÃO é uma Facade oficial do Laravel?

- A) Auth
- B) Route
- C) Request
- D) User

Explicação:

Auth, Route, e Request são todas Facades proeminentes no Laravel, fornecendo acesso aos sistemas de autenticação, roteamento e requisição HTTP, respectivamente. User, no entanto, não é uma Facade. User refere-se tipicamente à classe do modelo App\Models\User, que representa um registro na tabela de usuários. Você interage com ela usando o Eloquent, não através de uma interface de Facade.

Resposta Correta: D) User

#### Questão 21
Se você tem uma Facade MyFacade e chama um método que não existe nem na Facade nem na classe subjacente, qual erro será lançado?

- A) `\BadMethodCallException`
- B) `\ErrorException`
- C) `\ReflectionException`
- D) `\RuntimeException`

Explicação:

O fluxo é o seguinte:

A chamada estática MyFacade::nonExistentMethod() aciona Facade::__callStatic().

__callStatic resolve a instância do objeto subjacente do contêiner.

Ele então tenta chamar nonExistentMethod() na instância resolvida.

Como o método não existe no objeto, o PHP lançará uma \BadMethodCallException, indicando que o método que você tentou chamar não foi encontrado na classe do objeto.

Resposta Correta: A) \BadMethodCallException

#### Questão 22
O que o método estático Facade::clearResolvedInstances() faz?

- A) Limpa todos os dados de cache da aplicação.
- B) Remove todas as instâncias que foram resolvidas e armazenadas em cache pela classe Facade.
- C) Desconecta todas as conexões de banco de dados ativas.
- D) Limpa a sessão do usuário autenticado.

Explicação:

Para otimizar o desempenho, uma vez que uma Facade resolve sua instância subjacente do contêiner, ela armazena essa instância em um array estático local na própria classe da Facade. Em requisições subsequentes à mesma Facade dentro do mesmo ciclo de vida da aplicação, ela retornará a instância em cache em vez de resolvê-la novamente do contêiner. Facade::clearResolvedInstances() força a limpeza deste cache de instâncias resolvidas. Isso é usado principalmente internamente pelo framework durante os testes para garantir que cada teste comece com um estado limpo.

Resposta Correta: B) Remove todas as instâncias que foram resolvidas e armazenadas em cache pela classe Facade.

#### Questão 23
É possível ter uma Facade que resolva para um valor primitivo (como uma string ou um array) em vez de um objeto?

- A) Não, Facades devem sempre resolver para uma instância de objeto.
- B) Sim, mas apenas se o valor for registrado como um singleton no contêiner.
- C) Sim, mas você não poderá chamar nenhum método nela, pois não é um objeto.
- D) Não, o Service Container só pode armazenar objetos.

Explicação:

O Service Container do Laravel pode armazenar qualquer tipo de valor, incluindo strings, arrays, inteiros ou closures, não apenas objetos. Uma Facade pode ter um getFacadeAccessor que aponta para um binding que retorna um valor primitivo. No entanto, o propósito de uma Facade é chamar métodos em um objeto subjacente. Se a Facade resolvesse para uma string, por exemplo, qualquer tentativa de chamar um método nela (ex: MyFacade::someMethod()) resultaria em um erro fatal do PHP ("Call to a member function on string"). Portanto, embora tecnicamente possível, é funcionalmente inútil e contrário ao propósito de uma Facade.

Resposta Correta: C) Sim, mas você não poderá chamar nenhum método nela, pois não é um objeto.

#### Questão 24
Qual a relação entre Facades e o padrão de projeto "Facade" original (GoF - Gang of Four)?

- A) São exatamente a mesma coisa.
- B) Não têm nenhuma relação, o nome é apenas uma coincidência.
- C) As Facades do Laravel são uma implementação do padrão Facade, pois fornecem uma interface simplificada e unificada para um subsistema mais complexo (o Service Container e as classes de serviço).
- D) O padrão Facade original lida com interfaces estáticas, enquanto as Facades do Laravel lidam com instâncias dinâmicas.

Explicação:

As Facades do Laravel são, de fato, uma aplicação do padrão de projeto Facade. O padrão original descreve a criação de uma interface unificada para um conjunto de interfaces em um subsistema. Uma facade torna o subsistema mais fácil de usar. No Laravel, a classe Cache (a Facade) fornece uma API estática simples (Cache::get(), Cache::put()) que esconde a complexidade do sistema de cache subjacente, que envolve gerenciadores, diferentes drivers (Redis, file, etc.) e a resolução de instâncias do Service Container. Ela se encaixa perfeitamente na definição do padrão.

Resposta Correta: C) As Facades do Laravel são uma implementação do padrão Facade, pois fornecem uma interface simplificada e unificada para um subsistema mais complexo (o Service Container e as classes de serviço).

#### Questão 25
Considere o código de teste:

```
PHP

use Illuminate\Support\Facades\Route;
// ... dentro de um método de teste
Route::shouldReceive('get')->once()->with('test-url');

// Código que define a rota em routes/web.php:
// Route::get('test-url', ...);
```

Por que este teste, apesar de parecer correto, provavelmente não funcionará para verificar se uma rota foi definida?
- A) O método get da Facade Route não pode ser mockado.
- B) A Facade Route é resolvida muito cedo no ciclo de vida do framework, antes que o mock do teste seja aplicado, fazendo com que a chamada real não seja interceptada.
- C) shouldReceive só funciona para métodos que retornam um valor, e Route::get retorna void.
- D) O método with() não é compatível com a Facade Route.

Explicação:

Esta é uma nuance importante. Certas Facades do core, como Route e Config, são resolvidas e utilizadas muito cedo no processo de bootstrap da aplicação, antes mesmo do seu teste começar a ser executado. Quando você chama Route::shouldReceive() dentro do seu teste, a instância real do Router já foi resolvida e usada para carregar os arquivos de rota. O seu mock é aplicado tarde demais e nunca é atingido. A maneira correta de testar se uma rota existe e funciona é através de testes de HTTP (ex: $this->get('/test-url')->assertOk();), que interagem com a aplicação como um todo, em vez de tentar mockar a definição da rota.

Resposta Correta: B) A Facade Route é resolvida muito cedo no ciclo de vida do framework, antes que o mock do teste seja aplicado, fazendo com que a chamada real não seja interceptada.
