<script>
  const categories = Object.freeze([
    "setup",
    "migration",
    "model",
    "seeder",
    "controller",
  ]);
  const categoriesLabel = {
    setup: "Setup di progetto",
    migration: "Migration",
    seeder: "Seeder",
    model: "Modelli",
    controller: "Controller"
  };

  const steps = [
    {
      id: 1,
      category: "setup",
      title: "Inizializza il progetto",
      description: "",
      code: "composer create-project --prefer-dist laravel/laravel:^7.0 nome-progetto",
      checked: false,
    },
    {
      id: 2,
      category: "setup",
      title: "Crea il database tramite phpMyAdmin",
      description: "",
      code: "",
      checked: false,
    },
    {
      id: 3,
      category: "setup",
      title: "Modifica il file .env",
      description: "Copia il file env.example e rinomina solo il file copiato. In questo modo il file env.example continuerà ad essere presente nella repo, mentre il file .env non sarà pushato. Se non è presente, aggiungi .env al file .gitignore",
      code: "",
      checked: false,
    },
    {
      id: 4,
      category: "setup",
      title: "Avvia il progetto",
      description: "",
      code: "php artisan serve",
      checked: false,
    },
    {
      id: 5,
      category: "migration",
      title: "Crea le migration",
      description: "Nella funzione up delle migrations aggiungi i campi della tabella",
      code: "php artisan make:migration create_nometabella_table",
      checked: false,
    },
    {
      id: 6,
      category: "migration",
      title: "Esegui le migration",
      description: "Ricordati che le migration vengono eseguite in blocchi. Per annullare l'ultimo blocco, puoi eseguire il comando php artisan migrate:rollback",
      code: "php artisan migrate",
      checked: false,
    },
    {
      id: 7,
      category: "model",
      title: "Crea i model",
      description: "Ricordati che è preferibile usare il nome della tabella al singolare. Per tutti gli altri casi, puoi sfruttare la variabile protected $table dentro il modello per specificare il nome della tabella (ad es. modello Pizza, tabella Pizze e non Pizzas)",
      code: "php artisan make:model NomeModello",
      checked: false,
    },
    {
      id: 8,
      category: "seeder",
      title: "Importa il faker",
      description: "Aggiungiamo il faker (se ci serve) e importiamolo nel seeder tramite use Faker\Generator as Faker. Va aggiunto anche come parametro della funzione run",
      code: "composer remove fzaninotto/faker\ncomposer require fakerphp/faker",
      checked: false,
    },
    {
      id: 9,
      category: "seeder",
      title: "Crea i seeder",
      description: "",
      code: "php artisan make:seeder NometabellaTableSeeder",
      checked: false,
    },
    {
      id: 10,
      category: "seeder",
      title: "Esegui i seeder",
      description: "Se vuoi eseguire un solo seeder, aggiungi --class=NomeSeeder",
      code: "php artisan db:seed",
      checked: false,
    },
    {
      id: 11,
      category: "controller",
      title: "Crea il controller",
      description: "Ricordati che il comando --resource genera, oltre al controller, il modello e la cartella delle view (CRUD)",
      code: "php artisan make:controller --resource NomeController",
      checked: false,
    },
    {
      id: 12,
      category: "routes",
      title: "Crea le rotte",
      description: "Ricordati di controllare sempre le rotte generate con il comando php artisan route:list",
      code: "Route::resource('nomeTabella', 'NomeController')",
      checked: false,
    },
  ];
  let stepsCompleted = 0;

  function getStepsByCategory(category) {
    return steps.filter(step => step.category === category);
  }

  function updateStep(step) {
    if (step.checked) {
      stepsCompleted++;
    } else {
      stepsCompleted--;
    }
  }
  
  $: percStepsCompleted = ((stepsCompleted / steps.length) * 100).toFixed(2);
</script>

<main>
  <h1 class="title is-1">Bool Laravel Guide</h1>

  <progress class="progress is-info" value="{percStepsCompleted}" max="100">{percStepsCompleted}%</progress>

  <div class="steps">
    {#each categories as category}
      <h3 class="title is-3">{categoriesLabel[category]}</h3>
      {#each getStepsByCategory(category) as step}
        <div class="box">
          <label class="checkbox">
            <input type="checkbox" bind:checked={step.checked} on:change={() => updateStep(step)}>
            {step.title}
          </label>
          {#if step.description !== ""}
            <p class="description">{step.description}</p>
          {/if}
          {#if step.code !== ""}
            <pre class="code">{step.code}</pre>
          {/if}
        </div>
      {/each}
    {/each}
  </div>
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,200;0,400;0,700;1,200;1,400;1,700&display=swap");

  :global(body) {
    font-family: "Poppins", sans-serif !important;
  }

  main {
    width: 100%;
    height: 100vh;
    padding: 40px;
    background-color: #eee;
  }

  .steps {
    width: 100%;
    height: 80%;
    overflow-y: scroll;
  }

  .description {
    margin-top: 20px;
  }

  .code {
    margin-top: 20px;
  }
</style>
