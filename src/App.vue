<template>
  <div id="app">
    <h1>Splitter</h1>
    <div class="calculator">
      <div class="calculator-left">
        <div class="calculator-left__input">
          <label>Bill</label>
          <span class="validation-error" v-if="billValidation"
            >Can't be zero</span
          >
          <input v-model="bill" type="number" placeholder="0" />
          <svg
            class="calculator-left__input-svg"
            xmlns="http://www.w3.org/2000/svg"
            width="11"
            height="17"
          >
            <path
              fill="#9EBBBD"
              d="M6.016 16.328v-1.464c1.232-.08 2.22-.444 2.964-1.092.744-.648 1.116-1.508 1.116-2.58v-.144c0-.992-.348-1.772-1.044-2.34-.696-.568-1.708-.932-3.036-1.092V4.184c.56.144 1.012.4 1.356.768.344.368.516.816.516 1.344v.288h1.824v-.432c0-.448-.088-.876-.264-1.284a3.783 3.783 0 00-.744-1.116A4.251 4.251 0 007.54 2.9a5.324 5.324 0 00-1.524-.492V.872H4.288V2.36a5.532 5.532 0 00-1.416.324c-.448.168-.84.392-1.176.672-.336.28-.604.616-.804 1.008-.2.392-.3.844-.3 1.356v.144c0 .96.316 1.708.948 2.244.632.536 1.548.884 2.748 1.044v3.912c-.704-.16-1.248-.472-1.632-.936-.384-.464-.576-1.08-.576-1.848v-.288H.256v.576c0 .464.08.924.24 1.38.16.456.404.88.732 1.272.328.392.744.728 1.248 1.008s1.108.476 1.812.588v1.512h1.728zM4.288 7.424c-.688-.128-1.164-.332-1.428-.612-.264-.28-.396-.644-.396-1.092 0-.464.176-.832.528-1.104.352-.272.784-.448 1.296-.528v3.336zm1.728 5.712V9.344c.768.128 1.328.328 1.68.6.352.272.528.688.528 1.248 0 .544-.196.984-.588 1.32-.392.336-.932.544-1.62.624z"
            />
          </svg>
        </div>
        <div class="calculator-left__input">
          <label>Select Tip % </label>
          <div class="calculator-left__input-tips">
            <div
              @click="setSelectedTip(5)"
              class="tip-block"
              :class="{ active: selectedTip == 5 }"
            >
              5%
            </div>
            <div
              @click="setSelectedTip(10)"
              class="tip-block"
              :class="{ active: selectedTip == 10 }"
            >
              10%
            </div>
            <div
              @click="setSelectedTip(15)"
              class="tip-block"
              :class="{ active: selectedTip == 15 }"
            >
              15%
            </div>
            <div
              @click="setSelectedTip(25)"
              class="tip-block"
              :class="{ active: selectedTip == 25 }"
            >
              25%
            </div>
            <div
              @click="setSelectedTip(50)"
              class="tip-block"
              :class="{ active: selectedTip == 50 }"
            >
              50%
            </div>
            <input v-model="customTip" type="number" placeholder="Custom" />
          </div>
        </div>
        <div class="calculator-left__input">
          <label>Number of People</label>
          <span class="validation-error" v-if="peopleValidation"
            >Can't be zero</span
          >
          <input v-model="people" type="text" placeholder="0" />
          <svg
            class="calculator-left__input-svg"
            xmlns="http://www.w3.org/2000/svg"
            width="13"
            height="16"
          >
            <path
              fill="#9EBBBD"
              d="M9.573 7.729c.406 0 .784.07 1.126.209.342.14.639.33.881.569.232.227.438.503.614.82a5.1 5.1 0 01.407.949c.097.312.178.654.242 1.016.062.359.105.699.126 1.011.02.307.031.624.031.945 0 .836-.259 1.512-.768 2.01-.504.492-1.17.742-1.98.742H2.748c-.81 0-1.477-.25-1.98-.742C.259 14.76 0 14.084 0 13.248c0-.322.01-.64.032-.945.02-.312.063-.652.126-1.01.063-.363.144-.705.242-1.017.1-.323.238-.643.407-.948.176-.318.382-.594.613-.821.243-.238.54-.43.882-.57.342-.138.72-.208 1.125-.208.16 0 .313.067.61.265.183.123.397.264.636.421.204.134.48.259.822.372.333.11.671.167 1.005.167a3.19 3.19 0 001.006-.167c.341-.113.618-.238.822-.372l.636-.42c.296-.2.45-.266.61-.266zM6.598 0C7.63 0 8.522.38 9.252 1.129s1.1 1.666 1.1 2.724c0 1.06-.37 1.976-1.1 2.725-.73.75-1.623 1.13-2.654 1.13-1.03 0-1.924-.38-2.653-1.13-.73-.749-1.1-1.666-1.1-2.725 0-1.058.37-1.975 1.1-2.724C4.675.379 5.567 0 6.598 0z"
            />
          </svg>
        </div>
      </div>
      <div class="calculator-right">
        <div class="calculator-right__price">
          <div class="price-block">
            <div class="price-block__label">
              <label> Tip Amount </label>
              <span> / person </span>
            </div>
            <p>${{ tipAmount ? tipAmount : "0.00" }}</p>
          </div>
          <div class="price-block">
            <div class="price-block__label">
              <label> Total </label>
              <span> / person </span>
            </div>
            <p>${{ total ? total : "0.00" }}</p>
          </div>
        </div>
        <button @click="reset()">Reset</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      bill: null,
      people: null,
      customTip: null,
      selectedTip: null,
      billValidation: false,
      peopleValidation: false,
    };
  },
  methods: {
    setSelectedTip(value) {
      this.selectedTip = value;
    },
    reset() {
      this.bill = null;
      this.people = null;
      this.customTip = null;
      this.selectedTip = null;
    },
  },

  watch: {
    customTip(newVal) {
      if (newVal) {
        if (parseInt(newVal) <= 0 || parseInt(newVal) > 100) {
          this.selectedTip = null;
          this.customTip = null;
        } else {
          this.selectedTip = newVal;
        }
      }
    },
    bill(newVal) {
      if (parseInt(newVal) > 1000000) {
        this.bill = null;
      } else {
        if (parseInt(newVal) < 1) {
          this.billValidation = true;
        } else {
          this.billValidation = false;
        }
      }
    },
    people(newVal) {
      if (parseInt(newVal) > 100) {
        this.people = null;
      } else {
        if (parseInt(newVal) < 1) {
          this.peopleValidation = true;
        } else {
          this.peopleValidation = false;
        }
      }
    },
  },
  computed: {
    tipAmount() {
      if (
        this.bill &&
        this.selectedTip &&
        this.people &&
        !this.billValidation &&
        !this.peopleValidation
      ) {
        return (
          Math.round(
            ((parseFloat(this.bill) * this.selectedTip) /
              (100 * parseInt(this.people))) *
              100
          ) / 100
        );
      } else {
        return null;
      }
    },
    total() {
      if (
        this.bill &&
        this.selectedTip &&
        this.people &&
        !this.billValidation &&
        !this.peopleValidation
      ) {
        return (
          Math.round(
            ((parseFloat(this.bill) +
              (parseFloat(this.bill) * this.selectedTip) / 100) /
              parseInt(this.people)) *
              100
          ) / 100
        );
      } else {
        return null;
      }
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Space+Mono:wght@700&display=swap");
$strong-cyan: hsl(172, 67%, 45%);
$very-dark-cyan: hsl(183, 100%, 15%);
$dark-grayish-cyan: hsl(186, 14%, 43%);
$grayish-cyan: hsl(184, 14%, 56%);
$light-grayish-cyan: hsl(185, 41%, 84%);
$very-light-grayish-cyan: hsl(189, 41%, 97%);

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

::placeholder {
  color: $grayish-cyan;
  opacity: 1;
}

:-ms-input-placeholder {
  color: $grayish-cyan;
}

::-ms-input-placeholder {
  color: $grayish-cyan;
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type="number"] {
  -moz-appearance: textfield;
}

input {
  &:focus {
    outline: 2px solid $strong-cyan;
  }
}

#app {
  font-family: "Space Mono", monospace;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 80px;
  background: $light-grayish-cyan;
  min-height: 100vh;
  padding: 20px 0;
  @media only screen and (max-width: 899px) {
    gap: 40px;
  }
  h1 {
    font-size: 24px;
    word-break: break-all;
    word-wrap: break-word;
    width: 95px;
    letter-spacing: 9px;
    text-transform: uppercase;
    color: $very-dark-cyan;
  }

  .calculator {
    background: white;
    display: flex;
    flex-direction: row;
    border-radius: 20px;
    padding: 30px;
    min-width: 900px;
    min-height: 450px;
    gap: 30px;

    @media only screen and (max-width: 899px) {
      flex-direction: column;
      min-width: auto;
      min-height: auto;
      gap: 20px;
      padding: 25px;
    }

    &-left {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      gap: 25px;
      padding: 15px;

      @media only screen and (max-width: 899px) {
        padding: 10px;
      }

      &__input {
        display: flex;
        flex-direction: column;
        gap: 8px;
        position: relative;

        .validation-error {
          position: absolute;
          right: 0;
          color: red;
        }

        label {
          color: $dark-grayish-cyan;
        }

        &-svg {
          position: absolute;
          bottom: 14px;
          left: 17px;
        }

        &-tips {
          display: flex;
          flex-wrap: wrap;
          gap: 14px;

          .tip-block {
            font-size: 24px;
            color: white;
            flex: 28%;
            text-align: center;
            background: $very-dark-cyan;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 5px 0;
            height: 51px;
            cursor: pointer;
            transition: all 0.3s ease-in-out;

            @media only screen and (max-width: 899px) {
              flex: 42%;
            }

            &.active {
              color: $very-dark-cyan;
              background: $strong-cyan;
            }

            &:hover {
              color: $very-dark-cyan;
              background: $strong-cyan;
            }
          }

          input {
            border-radius: 8px;
            font-size: 22px !important;
            text-align: center !important;
            border: 0;
            font-weight: 700;
            flex: 28%;
            max-width: 108px;
            padding: 5px 0;
            background: $very-light-grayish-cyan;
            border-radius: 8px;
            height: 51px;
            color: $very-dark-cyan;
            font-family: "Space Mono", monospace;
            max-width: auto;
          }
        }

        input {
          text-align: right;
          border-radius: 8px;
          font-size: 24px;
          padding: 4px 14px;
          border: 0;
          font-weight: 700;
          color: $very-dark-cyan;
          background: $very-light-grayish-cyan;
          font-family: "Space Mono", monospace;
        }
      }
    }

    &-right {
      flex: 1;
      background: $very-dark-cyan;
      border-radius: 16px;
      min-height: 100%;
      padding: 40px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      gap: 30px;

      @media only screen and (max-width: 899px) {
        padding: 25px;
      }
      button {
        width: 100%;
        text-transform: uppercase;
        background: $strong-cyan;
        color: $very-dark-cyan;
        font-size: 20px;
        padding: 8px;
        border-radius: 6px;
        font-weight: 700;
        font-family: "Space Mono", monospace;
        border: 1px solid transparent;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
        &:hover {
          background: $very-dark-cyan;
          color: $strong-cyan;
          border: 1px solid $strong-cyan;
        }
      }

      &__price {
        display: flex;
        flex-direction: column;
        gap: 20px;
        .price-block {
          display: flex;
          justify-content: space-between;
          align-items: center;
          gap: 10px;
          &__label {
            display: flex;
            flex-direction: column;
            gap: 2px;
            label {
              color: white;
              font-size: 15px;
            }
            span {
              color: $grayish-cyan;
              font-size: 13px;
            }
          }
          p {
            font-size: 48px;
            color: $strong-cyan;

            @media only screen and (max-width: 899px) {
              font-size: 36px;
            }
          }
        }
      }
    }
  }
}
</style>
