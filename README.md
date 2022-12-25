## BIBLIOTECAS


## 1 - LARAVEL BREEZE

    sail composer require laravel/breeze --dev

    sail artisan breeze:install

    sail artisan migrate


## 2 - LIVEWIRE

    sail composer require livewire/livewire

Obs: Incluir os Styles e Scripts

...
@livewireStyles
</head>
<body>
... 
@livewireScripts
</body>
</html>


## 3 - TOASTR

<!-- Laravel Toastr ( https://github.com/yoeunes/toastr ) -->

    sail composer require yoeunes/toastr

    sail artisan vendor:publish --provider="Yoeunes\Toastr\ToastrServiceProvider"

Para utilizar basta incluir no método: toastr()->success('corpo_da_mensagem', 'título');
Para Otimizar a mensagem: config/toastr.php


## 4 - LOG VIEWER

<!-- Laravel Log viwer ( https://github.com/ARCANEDEV/LogViewer )  -->

1 - Modificar o arquivo .env LOG_CHANNEL=stack para LOG_CHANNEL=daily

    sail composer require arcanedev/log-viewer:9.x

2 - Em config/app.php incluir no array de providers:

'providers' => [
    ...
    Arcanedev\LogViewer\LogViewerServiceProvider::class,
],

3 - Em AppServiceProvider incluir no método boot:

use Illuminate\Pagination\Paginator;
 
Paginator::useBootstrap();

4 - Publicar o config:

    sail artisan log-viewer:publish

Para acessar os Logs: http://{nome_do_sistema}/log-viewer


## 5 - LARAVEL PT-BR

<!-- Laravel PT-BR ( https://github.com/lucascudo/laravel-pt-BR-localization ) -->

    sail composer require lucascudo/laravel-pt-br-localization --dev

    sail artisan vendor:publish --tag=laravel-pt-br-localization

** Altere Linha 85 do arquivo config/app.php para: 'locale' => 'pt-BR',
** Altere Linha 72 do arquivo config/app.php para: 'timezone' => 'America/Sao_Paulo',


## 5 - LARAVEL AUDITING

<!-- Laravel Audity  ( https://github.com/owen-it/laravel-auditing ) -->

    sail composer require owen-it/laravel-auditing

1 - Em config/app.php

'providers' => [

/*
* Package Service Providers...
*/

OwenIt\Auditing\AuditingServiceProvider::class,

]

    sail artisan vendor:publish --provider "OwenIt\Auditing\AuditingServiceProvider" --tag="config"

    sail artisan vendor:publish --provider "OwenIt\Auditing\AuditingServiceProvider" --tag="migrations" 

    sail artisan migrate

2 - Para monitorar uma Classe

use OwenIt\Auditing\Contracts\Auditable;  [**Importar_Esta_Classe**]

class nome_da_classe extends Model implements Auditable
{
    use \OwenIt\Auditing\Auditable; [**Importar_Esta_Classe**]

}


## 5 - LARAPEX CHARTS

<!-- https://github.com/ArielMejiaDev/larapex-charts -->

    sail composer require arielmejiadev/larapex-charts

    sail artisan vendor:publish --tag=larapex-charts-config

** Para criar um novo gráfico:

    sail artisan make:chart UsersGender

 Select a chart type:
  [0] Pie Chart
  [1] Donut Chart
  [2] Radial Bar Chart
  [3] Polar Area Chart
  [4] Line Chart
  [5] Area Chart
  [6] Bar Chart
  [7] Horizontal Bar Chart
  [8] Heatmap Chart
  [9] Radar Chart

  Ao selecionar será criada uma classe em app/Charts

  ** Para estanciar o gráfico na View: 

  1 - No Controller importar a classe criada e injetar no método que irá chamá-lo

    namespace App\Http\Controllers;

    use Illuminate\Http\Request;
    use App\Charts\UsersGender;

    class DashboardController extends Controller
    {
        public function index(UsersGender $chart)
        {
            return view('dashboard',compact('chart'));
        }
    }


2 - Na view:


    <div class="bg-white rounded shadow" style="width: 400px">
        {!! $chart->container() !!}    
    </div>

    <script src="{{ $chart->cdn() }}"></script>

    {{ $chart->script() }}


## Laravel DOMPDF 

<!-- https://github.com/barryvdh/laravel-dompdf -->

    sail composer require barryvdh/laravel-dompdf

1 - Adicionar o ServiceProvider no array de providers do arquivo config/app.php

    Barryvdh\DomPDF\ServiceProvider::class,

2 - Adicionar o atalho de facades no array de aliases do arquivo config/app.php

    'PDF' => Barryvdh\DomPDF\Facade::class,

3 - Publicar no config

    sail artisan vendor:publish --provider="Barryvdh\DomPDF\ServiceProvider"

4 - Criar o PDFController

    sail artisan make:controller PDFController

5 - Criar o método para mostrar o pdf de lista de usuarios no PdfController

    public function showUsers()
    {
        $users = User::all();
        $pdf = PDF::loadView('pdfs.show_users',compact('users'))->setPaper('a4','portrait');
        return $pdf->stream('lista_de_usuarios.pdf');
    }

** ** importante !!

- Os PDFs no DomPDF precisam de estilização na própria página
- lembrar de ativar a biblioteca GD
- Para a paginação funcionar , no config/dompdf.php alterar : "enable_php" => true,


## Laravel Spatie-Permissions

<!-- https://spatie.be/docs/laravel-permission/v5/introduction -->

** INSTALAÇÃO **

1 - Instalar Biblioteca:

    sail composer require spatie/laravel-permission

2 - Configurar o provider:

Em config/app.php

'providers' => [
    // ...
    Spatie\Permission\PermissionServiceProvider::class,
];

3 - Publicar as migrations:

    sail artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"

4 - Limpar o cache:

    sail artisan optimize:clear

5 - Rodar as Migrations:

    sail artisan migrate

** UTILIZAÇÃO **

1 - Adicionar ao User Model:


use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasRoles;
    // ...
}

2 - Para Criar uma Role ou Uma Permission:

use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;


$role = Role::create(['name' => 'Administrador']);
$permission = Permission::create(['name' => 'usuário-cadastrar']);

** Obs: Exemplo no AdminSeeder **



## Laravel Notifications ( https://github.com/mikebarlow/megaphone )

## Laravel Chatify ( https://github.com/munafio/chatify )
