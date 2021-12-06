<template>
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">{{character.name}}</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>


            <div class="modal-body">

              <!-- <pre>{{character}}</pre> -->

              <img :src="character.thumbnail"
              :alt="character.name"
              style="max-width: 100%">
            

              <div>

                <p class= "text-muted">
                  {{character.modified}}
                </p>
                <h5>Описание</h5>
                <p>{{character.description}}
                </p>

                  <!-- создали прохрждение по массиву, с доступом к свойствам через elComics -->

                <h5>Комиксы:</h5>
                <ul>
                  <li v-for="elComics in character.comics"
                  :key="elComics.name"> 
                    {{elComics.name}}
                  </li>
                </ul>

              </div>




            </div>


            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Закрыть</button>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
    export default {
      name: "AppModal",
      props: ["character"],//список параметров которые мы передаем в app modal в качестве атрибутов
    }
</script>

<style scoped>

</style>
