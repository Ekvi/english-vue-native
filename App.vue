<template>
  <view class="background">
    <view class="container">

      <view class="score">
        <view class="score-left">
          <text>Попыток всего</text>
          <text class="value">{{allAttempts}}</text>
        </view>
        <view class="score-right">
          <text>Правильно</text>
          <text class="value">{{correctAttempts}}</text>
        </view>
      </view>

      <view>
        <Picker :selectedValue="selectedCategory" :onValueChange="val => onChangeCategory(val)">
          <Item label="Категория..." value="" />
          <Item label="Глаголы" value="verb" />
          <Item label="Термины1" value="term1" />
          <Item label="Nginx" value="nginx" />
        </Picker>
      </view>

      <view class="place">
        <touchable-opacity
                class="word"
                :style="{backgroundColor: word.isSelected ? '#AEB6BF' : 'gainsboro'}"
                :on-press="() => setEnglish(word)"
                v-model="englishSelected"
                v-for="word in getEnglish"
        >
          <text>{{ word.en }}</text>
          <image :source="require('./assets/gb.png')"/>
        </touchable-opacity>
      </view>

      <button
              :onPress="shuffle"
              title="Get New"
              class="btn"
      />

      <view class="place">
        <touchable-opacity
                class="word"
                :style="{backgroundColor: word.isSelected ? '#AEB6BF' : 'gainsboro'}"
                :on-press="() => setRussian(word)"
                v-model="russianSelected"
                v-for="word in getRussian"
        >
          <text>{{ word.ru }}</text>
          <image :source="require('./assets/ru.png')"/>
        </touchable-opacity>

      </view>

    </view>
  </view>
</template>
<script>
  import _ from "lodash";
  import { Alert, Picker } from 'react-native';

  export default {
    components: {
      Picker,
    },
    data() {
      return {
        indexes: [],
        selectedCategory: null,
        englishSelected: null,
        russianSelected: null,
        wordsToShow: [],
        allAttempts: 0,
        correctAttempts: 0,
        words: []
      }
    },
    created() {
      this.words = this.getVerbs();
      this.selectedCategory = 'verb';
      this.shuffle();
    },
    computed: {
      getEnglish() {
        let words = this.wordsToShow.map(word => {
          return {id: word.id, en: word.en, isSelected: word.isSelected};
        });

        return _.shuffle(words);
      },
      getRussian() {
        let words = this.wordsToShow.map(word => {
          return {id: word.id, ru: word.ru, isSelected: word.isSelected};
        });

        return _.shuffle(words);
      },
    },
    methods: {
      setEnglish(word) {
        if (this.englishSelected != null) {
          this.englishSelected.isSelected = false;
        }

        word.isSelected = true;
        this.englishSelected = word;
        this.compare();
      },
      setRussian(word) {
        if (this.russianSelected != null) {
          this.russianSelected.isSelected = false;
        }

        word.isSelected = true;
        this.russianSelected = word;
        this.compare();
      },
      compare() {
        if (this.englishSelected == null || this.russianSelected == null) {
          return false;
        }

        if (this.englishSelected.id === this.russianSelected.id) {
          this.wordsToShow = this.wordsToShow.filter(item => {
            return item.id !== this.englishSelected.id;
          });

          this.clearActive();

          this.correctAttempts += 1;
          this.allAttempts += 1;
        } else {
          this.allAttempts += 1;
          Alert.alert(
                  '',
                  'Wrong, try again',
                  [
                    {text: 'OK', onPress: this.clearActive},
                  ],
                  { cancelable: false }
          );
        }
      },
      shuffle() {
        this.words = _.shuffle(this.words);

        this.wordsToShow = [];
        this.wordsToShow = this.words.slice(0, 7);

        this.correctAttempts = 0;
        this.allAttempts = 0;
      },
      clearActive() {
        if (this.englishSelected != null) {
          this.englishSelected.isSelected = false;
        }

        if (this.russianSelected != null) {
          this.russianSelected.isSelected = false;
        }

        this.englishSelected = null;
        this.russianSelected = null;
      },
      onChangeCategory(val) {
        this.selectedCategory = val;

        switch (val) {
          case 'verb':
            this.words = this.getVerbs();
            break;
          case 'term1':
            this.words = this.getTerms();
            break;
          case 'nginx':
            this.words = this.getNginx();
            break;
        }
      },
      getVerbs() {
        return [
          //{id: 1, en: 'to see', ru: 'видеть', isSelected: false},
          //{id: 2, en: 'to look', ru: 'смотреть', isSelected: false},
          //{id: 3, en: 'to hear', ru: 'слышать', isSelected: false},
          //{id: 4, en: 'to listen to', ru: 'слушать', isSelected: false},
          //{id: 5, en: 'to think', ru: 'думать', isSelected: false},
          {id: 6, en: 'to observe', ru: 'наблюдать', isSelected: false},
          {id: 7, en: 'to recognize', ru: 'узнавать', isSelected: false},
          //{id: 8, en: 'to feel', ru: 'чувствовать', isSelected: false},
          //d{id: 9, en: 'to imagine', ru: 'представлять себе', isSelected: false},
          //{id: 10, en: 'to watch', ru: 'следить', isSelected: false},
          {id: 11, en: 'to distinguish', ru: 'различать', isSelected: false},
          //{id: 12, en: 'to study', ru: 'изучать', isSelected: false},
          //{id: 13, en: 'to understand', ru: 'понимать', isSelected: false},
          //{id: 14, en: 'to train', ru: 'обучать', isSelected: false},
          //{id: 15, en: 'to teach', ru: 'обучать', isSelected: false},
          //{id: 16, en: 'to explain', ru: 'объяснять', isSelected: false},
          //{id: 17, en: 'to memorize', ru: 'заучивать', isSelected: false},
          {id: 18, en: 'to learn by heart', ru: 'учить наизусть', isSelected: false},
          //{id: 19, en: 'to remember', ru: 'помнить', isSelected: false},
          {id: 20, en: 'to bear in mind', ru: 'помнить', isSelected: false},
          //{id: 21, en: 'to know', ru: 'знать', isSelected: false},
          {id: 22, en: 'to mean', ru: 'значить', isSelected: false},
          //{id: 23, en: 'to forget', ru: 'забывать', isSelected: false},
          //{id: 24, en: 'to mistake', ru: 'ошибаться', isSelected: false},
          {id: 25, en: 'to examine', ru: 'проверять', isSelected: false},
          //{id: 26, en: 'to add', ru: 'складывать', isSelected: false},
          {id: 27, en: 'to subtract', ru: 'вычитать', isSelected: false},
          {id: 28, en: 'to multiply', ru: 'умножать', isSelected: false},
          {id: 29, en: 'to divide', ru: 'делить', isSelected: false},
          {id: 30, en: 'to count', ru: 'считать', isSelected: false},
          {id: 31, en: 'to calculate', ru: 'вычислять', isSelected: false},
          {id: 32, en: 'to express', ru: 'выражать', isSelected: false},
          //{id: 33, en: 'to describe', ru: 'описывать', isSelected: false},
          {id: 34, en: 'to relate', ru: 'рассказывать', isSelected: false},
          {id: 35, en: 'to retell', ru: 'пересказать', isSelected: false},
          //{id: 36, en: 'to translate', ru: 'переводить', isSelected: false},
          {id: 37, en: 'to consider', ru: 'рассматривать', isSelected: false},
          {id: 38, en: 'to suppose', ru: 'предполагать', isSelected: false},
          {id: 39, en: 'to expect', ru: 'ожидать', isSelected: false},
          {id: 40, en: 'to inquire', ru: 'расследовать', isSelected: false},
          {id: 41, en: 'to decide', ru: 'решать', isSelected: false},
          {id: 42, en: 'to research', ru: 'исследовать', isSelected: false},
          {id: 43, en: 'to discover', ru: 'открывать', isSelected: false},
          {id: 44, en: 'to find out', ru: 'выяснять', isSelected: false},
          {id: 45, en: 'to compare', ru: 'сравнивать', isSelected: false},
          //{id: 46, en: 'to prove', ru: 'доказывать', isSelected: false},
          {id: 47, en: 'to convince', ru: 'убеждать', isSelected: false},
          {id: 48, en: 'to conclude', ru: 'делать вывод', isSelected: false},
          {id: 49, en: 'to invent', ru: 'изобретать', isSelected: false},
          //{id: 50, en: 'to develop', ru: 'разработать', isSelected: false},
          {id: 51, en: 'to take into account', ru: 'принимать во внимание', isSelected: false},
          {id: 52, en: 'to make up one\'s mind', ru: 'настроиться', isSelected: false},
          {id: 53, en: 'to change one\'s mind', ru: 'передумать', isSelected: false},
          //{id: 54, en: 'to experiment', ru: 'экспериментировать', isSelected: false},
          //{id: 55, en: 'to analyze', ru: 'анализировать', isSelected: false},
          {id: 56, en: 'to process data', ru: 'обрабатывать данные', isSelected: false},
        ]
      },
      getTerms() {
        return [
            {id: 1, en: 'accept', ru: 'принимать, брать', isSelected: false},
            {id: 2, en: 'according', ru: 'в зависимости от', isSelected: false},
            {id: 3, en: 'accomplish', ru: 'выполнять, совершать', isSelected: false},
            {id: 4, en: 'adjust', ru: 'регулировать', isSelected: false},
            {id: 5, en: 'ambiguous', ru: 'неоднозначно', isSelected: false},
            {id: 6, en: 'ancestor', ru: 'предок', isSelected: false},
            {id: 7, en: 'anticipating', ru: 'опережающий', isSelected: false},
            {id: 8, en: 'approach', ru: 'подход , метод', isSelected: false},
            {id: 9, en: 'appropriate', ru: 'соответствующий,', isSelected: false},
            {id: 10, en: 'arbitrary', ru: 'произвольный, случайный', isSelected: false},
            {id: 11, en: 'assets', ru: 'средства', isSelected: false},
            {id: 12, en: 'assertion', ru: 'проверки', isSelected: false},
            {id: 13, en: 'assigned', ru: 'заданный, назначенный', isSelected: false},
            {id: 14, en: 'asterisk', ru: 'звездочка', isSelected: false},
            {id: 15, en: 'binding', ru: 'обязательный, связующий', isSelected: false},
            {id: 16, en: 'bootstrap', ru: 'начальная загрузка', isSelected: false},
            {id: 17, en: 'bound', ru: 'связанный', isSelected: false},
            {id: 18, en: 'capable', ru: 'работоспособный', isSelected: false}
        ]
      },
      getNginx() {
        return [
          {id: 1, en: 'scalability', ru: 'масштабируемость', isSelected: false},
          {id: 2, en: 'compression', ru: 'сжатие', isSelected: false},
          {id: 3, en: 'capability', ru: 'работоспособность', isSelected: false},
          {id: 4, en: 'consumption', ru: 'потребление, расход', isSelected: false},
          {id: 5, en: 'graceful', ru: 'изящный', isSelected: false},
          {id: 6, en: 'enhance', ru: 'усиливать, повышать', isSelected: false},
          {id: 7, en: 'accountability', ru: 'подотчетность', isSelected: false},
          {id: 8, en: 'comes to rescue', ru: 'приходит на помощь', isSelected: false},
          {id: 9, en: 'reduces', ru: 'уменьшать, снижать,', isSelected: false},
          {id: 10, en: 'availability', ru: 'доступность', isSelected: false},
          {id: 11, en: 'robust', ru: 'надёжный', isSelected: false},
          {id: 12, en: 'straightforward', ru: 'простой', isSelected: false},
          {id: 13, en: 'spawn', ru: 'порождать', isSelected: false},
          {id: 14, en: 'roughly', ru: 'приблизительно, грубо', isSelected: false},
          {id: 15, en: 'conjunction', ru: 'соединение', isSelected: false}
        ]
      },
    }
  }

</script>
<style>
  .background {
    background-color: #5D6D7E;
    height: 100%;
    width: 100%
  }
.container {
  background-color: #5D6D7E;
  /*background-color: #34495E;*/
  /*width: 100px;*/
  /*align-items: center;
  justify-content: center;*/
  /*display: flex;
  flex-wrap: wrap;*/
  /*flex: 1;*/
  /*display: flex;
  flex-direction: column;*/
  /*width: 100%;*/
  /*height: 100%;*/
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.score {
  display: flex;
  background-color: #D5DBDB;
  border-radius: 10px;
  flex-direction: row;
  min-height: 100px;
}
.score-left {
  flex: 2;
  justify-content: center;
  align-items: center;
  border-width: 1px;
  border-radius: 5px;
}
.score-right {
  flex: 2;
  justify-content: center;
  align-items: center;
  border-width: 1px;
  border-radius: 5px;
}
.value {
  font-size: 30px;
}
.place {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  background-color: #EBF5FB;
  border-width: 1px;
  border-radius: 5px;
  padding-top: 20px;
  padding-bottom: 20px;

  /*height: 32%;*/
  /*min-height: 200px;*/
  /*min-height: 200px;
  max-height: 200px;*/
  /*height: 200px;*/
  /*min-height: 30%;
  align-items:flex-start;
  align-content:flex-start;*/
  /*align-items:flex-start;
  align-content:flex-start;*/

}
.word {
  border-radius: 5px;
  padding: 10px;
  margin: 5px;
  display: flex;
  align-items: center;
}

</style>
