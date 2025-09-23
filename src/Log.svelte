<script>
    import { logs } from "./lib/stores";

    let date = '';
    let time = 0;
    let goal = 0;
    let activity = '';
    let mood = '';
    let notes = '';
    let goalweight = '';
    let weight = '';
    let goalsets = 0;
    let goalreps = 0;
    let sets = 0;
    let reps = 0;

    const listactivities = ["Walking", "Weights for Legs", "Weights for Back", "Weights for Glutes", "Weights for Arms", "Running", "Incline Threadmill"]; 

    function handleSubmit() {
        const entry = ({ date, time, goal, activity, mood, notes, goalweight, weight, goalsets,goalreps,sets,reps });
        logs.update(current => [...current,entry])
        emptylogs();

    }

    function emptylogs(){
        date = '';
        time = 0;
        goal = 0;
        activity = '';
        mood = '';
        notes = '';
        goalweight = '';
        weight = '';
        goalsets = 0;
        goalreps = 0;
        sets = 0;
        reps = 0;

    }

</script>

<main>
    <br><br>
    <h1>Enter Your Activity</h1>

    <h3>Pre-Workout Goals</h3>
    <input type="date" bind:value={date} /> <br> <br>

    <select bind:value={activity}>
        <option value="">-- Select an Activity --</option>
        {#each listactivities as option}
            <option value={option}>{option}</option>
        {/each}
    </select>
    <br><br>
    <input type="text" placeholder="If not in list, type your own" bind:value={activity} /> <br> <br>
    <p>What's the goal duration?</p>
    <input type="number" placeholder="Goal" bind:value={goal} min="0" max="200"> <br><br>
    <input type="range" placeholder="Goal" bind:value={goal} min="0" max="200"/> <br><br>
    <p>If lifting? How much weight?</p>
    <input type="number" placeholder="weights" bind:value={goalweight} min="0" max="120"> <br>
    <p>If lifting? How many goal sets?</p>
    <input type="number" placeholder="Sets" bind:value={goalsets} min="0" max="20"> <br>
    <p>If lifting? How many goal reps?</p>
    <input type="number" placeholder="Reps" bind:value={goalreps} min="0" max="20"> <br> <br>

    <h3>After Workout Stats</h3>
    <p>How long did you work out for?</p>
    <input type="number" placeholder="Duration" bind:value={time} min="0" max="200"> <br> <br>
    <input type="range" placeholder="Duration" bind:value={time} min="0" max="200"/> <br><br>
    <p>How much weight did you lift?</p>
    <input type="number" placeholder="weights" bind:value={weight} min="0" max="120"> <br>
    <p>How many sets?</p>
    <input type="number" placeholder="Sets" bind:value={sets} min="0" max="20"> <br>
    <p>How many reps/set?</p>
    <input type="number" placeholder="Reps" bind:value={reps} min="0" max="20"> <br> <br>
    <p>How do you feel after your workout?</p>
    <input type="text" placeholder="Mood" bind:value={mood} /> <br> <br>
    <p>Additional Notes</p>
    <textarea placeholder="Additional Notes" bind:value={notes}></textarea> <br>
    <br><br><button on:click={handleSubmit}>Submit</button> <br> <br>

    <div class="logentry">
        <h2>Your entry</h2>
        {#each $logs as entry, i}
        <h3>Entry {i+1}</h3>
        <h4>Date - {entry.date} </h4>
        <h4>Activity - {entry.activity}</h4>
        <h4>Mood - {entry.mood}</h4> 

        <p>Goal Duration: {entry.goal}</p>
        <p> Duration: {entry.time}</p> 
        <p>Goal Weight: {entry.goalweight}</p> <p>Weights: {entry.weight}</p>
        <p>Goal Sets: {entry.goalsets}</p> <p>Sets: {entry.sets} </p> 
        <p>Goal Reps: {entry.goalreps} </p> <p>Reps: {entry.reps}</p> <br>
        <p>Extra notes:</p>
        <p>{entry.notes}</p>
            
        {/each}


    </div>
    

</main>