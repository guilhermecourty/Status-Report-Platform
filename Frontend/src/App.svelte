<script>

let currentWeek = () => {
  new Promise((resolve, reject) => {
    var date = new Date();
    date.setHours(0, 0, 0, 0);
    // Thursday in current week decides the year.
    date.setDate(date.getDate() + 3 - (date.getDay() + 6) % 7);
    // January 4 is always in week 1.
    var week1 = new Date(date.getFullYear(), 0, 4);
    // Adjust to Thursday in week 1 and count number of weeks from date to week1.
    var result = 1 + Math.round(((date.getTime() - week1.getTime()) / 86400000 - 3 + (week1.getDay() + 6) % 7) / 7);
    resolve(result);
  })
  .then(result => result);
}

//está retornando certo, porém o svelte exibe antes, e responde undefined.
let week = currentWeek();


</script>

<main>
  <!-- <div class="column grey is-half is-offset-one-quarter"><p class="has-text-centered">W{week}</p></div> -->
<div class="columns">
  <div class="box column is-two-thirds is-offset-2">
      <div class="media-content">
        <div class="content">
          <p>
            <strong>W{week}</strong>
            <br>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean efficitur sit amet massa fringilla egestas. Nullam condimentum luctus turpis.
          </p>
        </div>
      </div>
  </div>
</div>

  

</main>


<style>

main{
  font-family: 'Lato', sans-serif;
}

</style>