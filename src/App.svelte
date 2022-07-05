<script>
  import { CATEGORIES_LABELS, CREATION_STEPS, CLONE_STEPS, USER_SELECTIONS, USER_SELECTION_LABELS } from "./setup/constraints";

  let steps = [];
  let stepsCompleted = 0;
  let userSelection = null;

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

  function loadSteps(selection) {
    userSelection = selection;
    if (userSelection === USER_SELECTIONS.CREATION) {
      steps = JSON.parse(JSON.stringify(CREATION_STEPS));
    } else {
      steps = JSON.parse(JSON.stringify(CLONE_STEPS));
    }
    stepsCompleted = 0;
  }
  
  $: percStepsCompleted = Math.round((stepsCompleted / steps.length) * 100);
  $: categories = Array.from(new Set(steps.map(step => step.category)));
</script>

<section class="hero is-link">
  <div class="hero-body">
    <p class="title is-1">Laravel Steps</p>
    <p class="subtitle">
      La guida passo passo per la creazione / clone di un progetto Laravel
    </p>
  </div>
</section>

<section class="main section">
  <div class="user-selection">
    <button class="selection" class:active={userSelection === USER_SELECTIONS.CREATION} on:click={() => loadSteps(USER_SELECTIONS.CREATION)}>
      {USER_SELECTION_LABELS.CREATION}
    </button>
    <button class="selection" class:active={userSelection === USER_SELECTIONS.CLONE} on:click={() => loadSteps(USER_SELECTIONS.CLONE)}>
      {USER_SELECTION_LABELS.CLONE}
    </button>
  </div>

  {#if userSelection}
    <div class="progress-wrapper">
      <h3 class="title is-3">{USER_SELECTION_LABELS[userSelection]} ({stepsCompleted} su {steps.length})</h3>
      <progress
        class="progress is-info"
        value="{percStepsCompleted ? percStepsCompleted : 1}"
        max="100"
      >
        {percStepsCompleted}%
      </progress>
    </div>

    <div class="steps">
      {#each categories as category}
        <h3 class="category title is-3">{CATEGORIES_LABELS[category]}</h3>
        {#each getStepsByCategory(category) as step}
          <div class="box">
            <label class="checkbox">
              <input type="checkbox" bind:checked={step.checked} on:change={() => updateStep(step)}>
              <strong>{step.title}</strong>
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
  {/if}
</section>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,200;0,400;0,700;1,200;1,400;1,700&display=swap");

  :global(body) {
    width: 100%;
    height: 100vh;
    font-family: "Poppins", sans-serif;
    color: #001632;
    background-color: rgb(236, 236, 236);
  }

  .user-selection {
    margin-bottom: 40px;
    display: flex;
    justify-content: center;
  }

  .selection {
    height: 80px;
    margin: 0 20px;
    padding: 16px 24px;
    border: 4px solid transparent;
    border-radius: 6px;
    background-color: #00de6b;
    font-size: 22px;
    color: #001632;
    font-weight: 700;
    cursor: pointer;
  }

  .selection:hover {
    border: 4px solid #00de6b;
    background-color: transparent;
    transition: .5s;
  }

  .selection.active {
    border: 4px solid #00de6b;
    background-color: transparent;
    transition: .5s;
  }

  .progress-wrapper {
    height: 120px;
  }

  .main {
    padding: 40px;
    background-color: rgb(236, 236, 236);
  }

  .title {
    font-weight: 700;
  }

  .category:before {
    content: '';
    display: inline-block;
    width: 20px;
    height: 20px;
    background-color: #00de6b;
    margin-right: 10px;
  }

  .description {
    margin-top: 20px;
  }

  .code {
    margin-top: 20px;
  }
</style>
