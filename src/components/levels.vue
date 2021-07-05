<template>
      <div class="info-game__level level">
            <h3>
                  Выберете уровень
            </h3>
            <ul class="level__list">
                  <li class="level__item" v-for="(level, index) in levels" :key="index">
                        <label>
                              <input type="radio" name="level" :value="level.time" v-model="time">
                              <span>{{level.name}}</span>
                        </label>
                  </li>
            </ul>
      </div>
</template>

<script>
export default {
      data() {
            return {
                  levels: [
                        {
                              name: 'Легкий',
                              time: 1500
                        },
                        {
                              name: 'Средний',
                              time: 1000
                        },
                        {
                              name: 'Сложный',
                              time: 400
                        }
                  ],
                  time: 1500,
            }
      }
}
</script>

<style scoped>
.level {
      margin-top: 30px
}

.level__list {
      display: -webkit-box;
      display: -ms-flexbox;
      display: flex;
      -webkit-box-orient: vertical;
      -webkit-box-direction: normal;
            -ms-flex-direction: column;
                  flex-direction: column;
}

.level__item:not(:last-child) {
      margin-bottom: 5px
}
</style>