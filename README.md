Design Patterns Estruturais
==========================
 * [Adapter](#-adapter)
 * [Bridge](#-bridge)
 * [Composite](#-composite)
 * [Decorator](#-decorator)
 * [Facade](#-facade)
 * [Flyweight](#-flyweight)
 * [Proxy](#-proxy)

🔌 Adapter
-------
Exemplo do mundo real
> Considere que você tem algumas fotos em seu cartão de memória e que você precisa transferi-los para o seu computador. E para transferi-los você precisa de algum tipo de adaptador que seja compatível com as portas do computador. Neste caso o leitor de cartões é um adaptador.
> Outro exemplo seria o famoso adaptador de alimentação; Um plugue de três pernas não pode ser conectado a uma saída de duas pontas, ele precisa usar um adaptador de energia que torna compatível com a saída de duas pontas.
> Outro exemplo seria um tradutor traduzindo palavras ditas por uma pessoa a outra.

Resumindo
> O padrão Adapter permite que você envolva um objeto incompatível em um adaptador para torná-lo compatível com outra classe.

A Wikipidia diz
> Na engenharia de software, o padrão do Adapter é um padrão de design de software que permite que a interface de uma classe existente seja usada como outra interface. É freqüentemente usado para fazer com que as classes existentes trabalhem com outras sem modificar seu código-fonte.

**Exemplo Programático**

Considere um jogo onde há um caçador e ele caça leões.

Primeiro temos uma interface `Lion` que todos os tipos de leões têm que implementar.

```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```
E o caçador espera que qualquer implementação da interface `Lion` para caçar.
```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

Agora vamos dizer que temos de adicionar um `WildDog` no nosso jogo para que o caçador possa caçar isso também. Mas não podemos fazer isso diretamente porque o cão tem uma interface diferente. Para torná-lo compatível para o nosso caçador, vamos ter que criar um adaptador compatível.
 
```php
// Isso precisa ser adicionado ao jogo
class WildDog
{
    public function bark()
    {
    }
}

// Adaptador em torno do cão selvagem para torná-lo compatível com o nosso jogo
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
E agora o `WildDog` pode ser usado em nosso jogo usando` WildDogAdapter`.

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 Bridge
------
Exemplo do mundo real
> 
Considere que você tem um site com páginas diferentes e supostamente o usuário pode alterar o tema. O que você faria? Criaria várias cópias de cada uma das páginas para cada um dos temas ou você apenas criaria o tema separado e carregar com base nas preferências do usuário? Bridge permite que você faça a segunda opção.

![Com e sem o Bridge](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

Resumindo
> Bridge pattern é sobre preferir a composição sobre herança. Detalhes de implementação são empurrados de uma hierarquia, para outro objeto com uma hierarquia separada.

Wikipedia diz
> O bridge é um padrão de projeto usado na engenharia de software que destina-se a "desacoplar uma abstração de sua implementação para que os dois possam variar independentemente".

**Exemplo Programático**

Traduzindo nosso exemplo de WebPage acima. Aqui temos a hierarquia `WebPage`

```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "Careers page in " . $this->theme->getColor();
    }
}
```
E a hierarquia de tema separada
```php

interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return 'Dark Black';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return 'Off white';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return 'Light blue';
    }
}
```
E ambas as hierarquias
```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 Composite
-----------------

Exemplo do mundo real
> Toda organização é composta por empregados. Cada um dos empregados possuirá as mesmas caracteristicas, tais como salário, possui responsabilidades, pode ou não ter que se reportar a algém ou pode ou não ter subordinados.

Resumindo
> Composite Pattern deixa o cliente tratar objetos individuais de maneira uniforme.

Wikipedia diz
> Em engenharia de software, o Composite Patter é um design pattern de particionamento. O Composite Pattern descreve que um grupo de objetos deverá ser tratado da mesma maneira que apenas uma instancia de um objeto. A intensão de um Composite é "compor" objetos em estrutura de arvore representando parte de uma hierarquia. Implementar o Composite Patter é permitir que clientes tratem objetos individuais e composições de maneira uniforme.

**Exemplo programático**

Usaremos nosso exemplo acima com funcionários. Temos diferentes tipos de funcionários.

```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

Então, temos uma organização que é composta por inumero tipos diferentes de funcionários.

```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

E ele pode ser usado como.

```php
// Prepara os funcionários
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// Adicione-os à organização
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Net salaries: " . $organization->getNetSalaries(); // Net Salaries: 27000
```

☕ Decorator
-------------

Exemplo do mundo real

> Imagine você gerenciando uma oficina de carro com muitos serviços. Agora, como você calcularia a conta a ser cobrada? Você escolhe um serviço e começa a adicionar a este serviço preços por demais serviços prestados, de maneira dinâmica, até você ter o custo total final. Aqui que cada um destes serviços seria um decorator.

Resumindo
> Decorator Pattern permite que você altere o comportamento de um objeto em tempo de execução, envolvendo-o por um objeto de uma classe Decorator. 

Wikipedia diz
> Em programação orientada a objeto, o Decorator Pattern permite que se adicione comportamentos para um objeto, tanto de maneira estatica quanto dinâmica, sem afetar o comportamento de outros objetos da mesma classe. O Decorator Pattern é muito útil para ser aderente ao principio de responsabilidade única, uma vez que ele permite que funcionalidades sejam divididas entre classes que compartilhe uma única preocupação.

**Exemplo programático**
Vamos pegar um café como exemplo. Primeiro vamos temos um SimpleCoffee implementando a interface Coffee.

```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return 'Simple coffee';
    }
}
```
Vamos pegar o café como exemplo. Primero de tudo, nós temos um simples café implementando a interface café.
```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```
Vamos fazer um café agora!
```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```

📦 Facade
----------------

Exemplo do mundo real
> Como voce liga seu compudator? "Aperto o botão ligar", você diz! Isto é o que você pensa, já que está utilizando uma simples interface exposta pelo computador. Internamente ele faz muitas coisas para que o "ligar" ocorra. Esta interface simples para um subsistema complexo é o Facade.

Resumindo
> O Pattern Facade provê uma interface simples para um subsistema complexo.

Wikipedia diz
> Uma Facade é um objeto que provê uma interface simples para uma grande porção de código como uma Class Library.

**Exemplo programatico**
Usando o exemplo sobre o computador acima, vamos criar uma classe para representa-la.

```php
class Computer
{
    public function getElectricShock()
    {
        echo "Ouch!";
    }

    public function makeSound()
    {
        echo "Beep beep!";
    }

    public function showLoadingScreen()
    {
        echo "Loading..";
    }

    public function bam()
    {
        echo "Ready to be used!";
    }

    public function closeEverything()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth()
    {
        echo "Zzzzz";
    }

    public function pullCurrent()
    {
        echo "Haaah!";
    }
}
```
Aqui temos uma Facade
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```
Como usar a classe Facade.
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```


🍃 Flyweight
---------

Exemplo do mundo real
> Você já tomou chá fresco de alguma barraca? Eles geralmente fazem mais que um copo que você solicitou e guardam o que restou para servir a outro cliente que também solicitar, assim economizam recursos, como por exemplo gás. Flyweight Pattern é sobre isto: compartilhar.


Resumindo
> Ele é usado para minimizar o uso de memoria ou gasto computacional, compartilhando o máximo que der com objetos semelhantes.


O Wikipedia diz
> Em programa de computadores, Flyweight é um padrão de design de software. Um Flyweight Pattern é um objeto que minimiza o uso de memória, compartilhando o máximo possível de indormações com objetos similares; Esta é uma maneira de usar objetos em larga escala quando uma simples representação repedida poderia usar uma iniceitavel quantidade de memória.

**Exemplo programatico**
Traduzindo o nosso exemplo acima sobre o chá. Primeiro de tudo, temos tipos de chá e preparadores de chá.

```php
// Quanquer coisa que for cacheada é Flyweight
// Tipos de chá será Flyweight
class KarakTea
{
}

// Atua como uma factory e salva os chás
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```
Então, nós temos o 'TeaShop' o qual pega os pedidos e os serve
```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "Servindo chá para a mesa# " . $table;
        }
    }
}
```
E isto pode ser usado como no caso a baixo

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Servindo chá para a mesa #1
// Servindo chá para a mesa #2
// Servindo chá para a mesa #5
```

🎱 Proxy
-------------------
Exemplo do mundo real 
> Você já utilizou um cartão de acesso para passar por uma porta? Existem muitas opções para abrir uma porta, por exemplo, você poderia tanto usar um cartão de aecsso quanto apertar um botão para passar por sua segurança. A função principla desta porta é abrir, porém existe um proxy antes disto que adiciona outras funcionalidades. Deixe-me explicar melhor isto usando o exemplo em código a seguir.

Resumindo
> Usando o Proxy Patter, uma classe irá representar a funcionalidade de outra classe.

Wikipedia diz
> Um Proxy, de forma geral, é uma classe funcionando como uma interface para outra coisa. Um Proxy é um envolucro (wrapper) ou Agent Object que é chamado pelo cliente para acessar o objeto real por traz da cena. O uso de Proxy pode ser simplesmente para repassar para um objeto real ou então adicionar lógica a ele. Na lógica adicional que o Proxy pode adicionar temos por exemplo cache para operações que consomem muitos recursos.

**Exemplo Programatico**
Tendo o exempo da porta a cima, primeiramnete precisaremos criar a interface de porta e implementa-la.

```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "Abrindo a porta";
    }

    public function close()
    {
        echo "Fechando a porta";
    }
}
```
Então, nós temos um Proxy para adicionar segurança a porta que quisermos.
```php
class SecuredDoor implements Door
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "Não é possivel";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```
E assim é como ele pode ser utilizado
```php
$door = new SecuredDoor(new LabDoor());
$door->open('invalid'); // Não é possivel

$door->open('$ecr@t'); // Abrindo a Porta
$door->close(); // Fechando a porta
```
