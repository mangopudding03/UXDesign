<script>
    import { logs } from "./lib/stores";

    let editIndex = null;
    let editEntry = { date:'',
        time: 0,
        goal: 0,
        activity: '',
        mood : '',
        notes : '',
        goalweight:'',
        weight: '',
        goalsets : 0,
        goalreps : 0,
        sets : 0,
        reps : 0};

    function editLog(ei){
        editIndex = ei;
        editEntry = { ...$logs[ei] }; // copies the log entry that needs to be edited withiut directly mutating the actual thing
    }

    function saveEdit(){
        logs.update(current => {
            const updated = [...current]; //copies the existing log entry into updated
            updated[editIndex] = { ...editEntry }; // spread operator to append the edited entry with the rest of logs
            return updated;
        });
        editIndex = null;
    }

    function cancelEdit(){
        editIndex = null;
    }
</script>

<main>
    <br><h1>Your previous logs</h1>
    {#if $logs.length === 0}
        <h3>No previous logs</h3>
    {:else}
    
        {#each $logs as entry, i}
            {#if editIndex === i}
            <div class="prevlog">
                <p>Edit Date</p>
                <input type="date" bind:value={editEntry.date} /> <br> <br>
                <p>Edit Activity</p>
                <input type="text" placeholder="Activity" bind:value={editEntry.activity} /> <br> <br>
                <p>Edit goal duration</p>
                <input type="number" placeholder="Goal" bind:value={editEntry.goal} min="0" max="200"> <br>
                <p>Edit weights</p>
                <input type="number" placeholder="weights" bind:value={editEntry.goalweight} min="0" max="120"> <br>
                <p>Edit goal sets</p>
                <input type="number" placeholder="Sets" bind:value={editEntry.goalsets} min="0" max="20"> <br>
                <p>Edit goal reps</p>
                <input type="number" placeholder="Reps" bind:value={editEntry.goalreps} min="0" max="20"> <br> <br>

                <h3>After Workout Stats</h3>
                <p>Edit workout duration?</p>
                <input type="number" placeholder="Duration" bind:value={editEntry.time} min="0" max="200">
                <p>Edit weights</p>
                <input type="number" placeholder="weights" bind:value={editEntry.weight} min="0" max="120"> <br>
                <p>Edit sets</p> 
                <input type="number" placeholder="Sets" bind:value={editEntry.sets} min="0" max="20"> <br>
                <p>Edit reps/set</p> 
                <input type="number" placeholder="Reps" bind:value={editEntry.reps} min="0" max="20"> <br> <br>
                <p>How did you feel after your workout?</p>
                <input type="text" placeholder="Mood" bind:value={editEntry.mood} /> <br> <br>
                <p>Additional Notes</p>
                <textarea placeholder="Additional Notes" bind:value={editEntry.notes}></textarea> <br>
                <button on:click={saveEdit}>Save Edit</button>
                <button on:click={cancelEdit}>Cancel</button>

            </div>
                
            {:else}
            <div class="prevlog">
                <h3>Entry {i+1}</h3> 
                <h4>Date - {entry.date}</h4>
                <h4>Activity - {entry.activity}</h4>
                <h4>Mood - {entry.mood}</h4> <br>
                <p>Goal Duration: {entry.goal}</p>
                <p> Duration: {entry.time}</p> <br>
                <p>Goal Weight: {entry.goalweight}</p> <p>Actual Weight: {entry.weight}</p>
                <p>Goal Sets: {entry.goalsets}</p> <p>Actual Sets: {entry.sets}</p>
                <p>Goal Reps: {entry.goalreps}</p>  <p>Actual Reps: {entry.reps}</p> <br>
                <p>Extra Notes</p>
                <p>{entry.notes}</p>
                <button on:click={() => editLog(i)}>Edit</button>

            </div>

            {/if}
        {/each}
    {/if}
</main>
