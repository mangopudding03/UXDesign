<script>
  import { logs } from './lib/stores';
  import Router from 'svelte-spa-router'
  import LogYourDay from './Log.svelte'
  import PreviousLogs from './PrevLog.svelte'
  import Home from './App.svelte'

  // Nav routes
  const routes = {
    '/' : Home,
    '/Log': LogYourDay,
    '/PrevLog': PreviousLogs
  }
  let name = 'Maanya';
  let joindate = '05-08-2025';
  let currentdate = new Date();
  let welcomemsg = true;

  function greet() {
    if (name) {
      return 'Welcome to ' + name + "'s " + 'Journal!';
    }
    return 'Hello!';
  }

  function handleNav() {
    welcomemsg = false;
  } 


  function datediff(inputdate) {
    if (!inputdate) return 0;
    let join = new Date(inputdate);
    let diffTime = currentdate.getTime() - join.getTime();
    let diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
    return diffDays;
  }

  // For reactivity, svelte uses $ so that it changes 
  $: activitySummary = (() => {
    const summary = {};
    const dates = new Set();
    $logs.forEach(entry => {
      dates.add(entry.date);
      const act = entry.activity; // for all summary data in each activity 
      if (!summary[act]) {
        summary[act] = {
          totalTime: 0,
          totalGoalTime: 0,
          totalGoalSets: 0,
          totalGoalReps: 0,
          totalSets: 0,
          totalReps: 0,
          count: 0
        };
      }

    //Totals calculations, convert it into number with +
      summary[act].totalTime += +entry.time;
      summary[act].totalGoalTime += +entry.goal;
      summary[act].totalGoalSets += +entry.goalsets;
      summary[act].totalGoalReps += +entry.goalreps;
      summary[act].totalSets += +entry.sets;
      summary[act].totalReps += +entry.reps;
      summary[act].count += 1;
    });

    //Averages
    for (const act in summary) {
      const s = summary[act];
      s.avgTime = s.count > 0 ? Math.round(s.totalTime / s.count) :0;
      s.avgGoalTime = s.count > 0 ? Math.round(s.totalGoalTime / s.count) :0;
      s.avgSets = s.count > 0 ? Math.round(s.totalSets / s.count) :0;
      s.avgGoalSets = s.count > 0 ? Math.round(s.totalGoalSets / s.count) :0;
      s.avgReps = s.count > 0 ? Math.round(s.totalReps / s.count) :0;
      s.avgGoalReps = s.count > 0 ? Math.round(s.totalGoalReps / s.count) :0;
    }

    return {summary, totalDays: dates.size};
  })();
</script>

<nav class="nav">
      <a href="#/" on:click={() => welcomemsg = true}>Home</a>
      <a href="#/Log" on:click={handleNav}>Log Your Day</a>
      <a href="#/PrevLog" on:click={handleNav}>Previous Logs</a>
</nav>


<main>

  {#if welcomemsg}
    <div class="username">
      <h1>
        {greet()}
      </h1>
      <h2>Today is {new Date().toLocaleDateString()}</h2>
      <h2>Enrolled since {datediff(joindate)}
          {datediff(joindate) === 1 ? "day" : "days!"}</h2>
      <h2>Active since {activitySummary.totalDays} {activitySummary.totalDays === 1 ? "day" : "days!"} </h2>
    </div>

    <a href="#/Log" on:click={handleNav}>
      <button class="LogPageButton"> Log Page</button>
    </a>
    <br><br>

    <div class="ActivitySummary">
      {#each Object.entries(activitySummary.summary) as [activity, calc]}
        <h3 class="ActivityTitle"> <i>{activity}</i></h3>
        <h3> Goal: {calc.avgGoalTime} minutes</h3>
        <p> <strong>Average Duration:</strong> {calc.avgTime} minutes</p>
        <h3>Goal Sets: {calc.avgGoalSets}</h3>
        <p> <strong>Average Sets: </strong> {calc.avgSets} </p> 
        <h3>Goal Reps: {calc.avgGoalReps}</h3>
        <p> Average Reps: {calc.avgReps}</p>
      {/each}

    </div>
    
    
  {:else}
    <Router {routes} {logs}/>
  {/if}

  



</main>


