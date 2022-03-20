<template>
  <div class="presentation">
    <p>
    For this test, i respect what is asked :
      <ol>
        <li><b>Fetch the patient cards using our local server :</b> i did a fetch to get datas and parse it</li>
        <li><b>Display the patient cards in the corresponding column (Pending + Rejected and Done)</b> : I made a component to show a list and invoke it twice. First one with a computed filtering cards with "Pending" & "rejecting" status. For the second one, same method but only with "done" cards</li>
        <li><b>Allow user to change the card status from Pending or Rejected to Done and from Done to Rejected : </b> This is possible thanks the library vuedraggable. I added it to simplify drag & drop operations. On each movements, i update the status & position of the element on the list</li>
        <li><b>Add a local filtering by patient name and arrhythmias :</b>i update the list whom serves me to generate the 2-columns datas with the provided filter</li>
      </ol>
    </p>
    <p>
      but i also add some things :
        <ul>
        <li>each patient card looks like a post-it note</li>
        <li>i add a rotate transformation on this element to present it like a real post-it</li>
        <li>the title of the column is sticky</li>
        <li>During a filtering, i underline the text matching the search</li>
        <li>I parse the "created date" attribute and shows it localized</li>
        <li>Add this comment on my work and it's hideable according the width of the page ( width &#60; 1200px )</li>
      </ul>
    </p>
    <p>
      <b>~ 5 hours for this result</b>
    </p>
  </div>
</template>

<script>
export default {
  name: 'PresentationTest',
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .presentation {
    text-align: left;
    top: 0px;
    position: sticky;
  }
</style>
