<template>
  <section class="container">
    <div class="people-num">
      <div v-for="(account, index) in accounts" :key="`first-${index}`">
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <div v-if="account.level < 6">
              <label class="label level-lavel">{{ account.level }}等級</label>
            </div>
            <div v-else>
              <label class="label level-lavel"
                >V{{ account.level - 6 }}等級</label
              >
            </div>
          </div>
          <div class="field-body">
            <div class="field">
              <p class="control">
                <input
                  v-model="account.num"
                  class="input"
                  type="number"
                  min="0"
                  placeholder="0"
                />
              </p>
            </div>
          </div>
        </div>
      </div>
      <button
        class="button is-primary"
        style="margin-top: 12px; margin-bottom: 12px;"
        @click="addPartTimeWorker()"
      >
        Add a part-time worker
      </button>
      <div
        v-for="(partTimeWorker, index) in partTimeWorkers"
        :key="`second-${index}`"
        style="border: 1px solid #9b9b9b; box-sizing: border-box; padding: 1rem; border-radius: 15px;"
      >
        <div class="field is-horizontal">
          <div class="field-label is-normal">
            <label for="" class="label">No.{{ index }}</label>
          </div>
          <div>
            <div class="field is-horizontal">
              <div class="field-label is-normal">
                <label for="" class="label">時給</label>
              </div>
              <div class="field-body">
                <div class="field">
                  <p class="control">
                    <input
                      v-model="partTimeWorker.salary"
                      type="number"
                      class="input"
                      min="0"
                      placeholder="0"
                    />
                  </p>
                </div>
              </div>
            </div>
            <div class="field is-horizontal">
              <div class="field-label is-normal">
                <label for="" class="label">人数</label>
              </div>
              <div class="field-body">
                <div class="field">
                  <p class="control">
                    <input
                      v-model="partTimeWorker.num"
                      type="number"
                      class="input"
                      min="0"
                      placeholder="0"
                    />
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="main-content">
      <p>{{ toHms(timer) }}</p>
      <p>{{ Math.round(money) }}円</p>
      <button class="button is-primary is-large" @click="startTimer">
        Start
      </button>
      <button class="button is-primary is-large" @click="stopTimer">
        Stop
      </button>
      <button class="button is-primary is-large" @click="ResetTimer">
        Reset
      </button>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      timer: 0,
      money: 0,
      accounts: [
        { level: 1, salary: 2135, profitableSalary: 10000, num: 0 },
        { level: 2, salary: 2396, profitableSalary: 10300, num: 0 },
        { level: 3, salary: 2682, profitableSalary: 10700, num: 0 },
        { level: 4, salary: 2969, profitableSalary: 11100, num: 0 },
        { level: 5, salary: 3385, profitableSalary: 11700, num: 0 },
        { level: 6, salary: 4010, profitableSalary: 12600, num: 0 },
        { level: 7, salary: 4583, profitableSalary: 13500, num: 0 },
        { level: 8, salary: 5208, profitableSalary: 14300, num: 0 },
        { level: 9, salary: 5990, profitableSalary: 15500, num: 0 },
        { level: 10, salary: 6875, profitableSalary: 16700, num: 0 },
        { level: 11, salary: 7813, profitableSalary: 18100, num: 0 }
      ],
      partTimeWorkers: []
    }
  },
  methods: {
    startTimer() {
      this.intervalId = setInterval(() => {
        // 処理内容
        this.timer += 1

        this.accounts.forEach(employee => {
          this.money += (employee.salary * employee.num) / 3600
        })

        if (this.partTimeWorkers.length > 0) {
          this.partTimeWorkers.forEach(partTimeWorker => {
            this.money += (partTimeWorker.salary * partTimeWorker.num) / 3600
          })
        }
      }, 1000) // 1秒間隔で処理
    },
    stopTimer() {
      clearInterval(this.intervalId)
    },
    ResetTimer() {
      this.timer = 0
      this.money = 0
    },
    toHms(t) {
      let hms = ''
      const h = (t / 3600) | 0
      const m = ((t % 3600) / 60) | 0
      const s = t % 60

      if (h !== 0) {
        hms = padZero(h) + ':' + padZero(m) + ':' + padZero(s)
      } else if (m !== 0) {
        hms = '00:' + padZero(m) + ':' + padZero(s)
      } else {
        hms = '00:' + '00:' + padZero(s)
      }

      return hms
    },
    sliceMoney(money) {
      return this.money.slice(0, 4)
    },
    addPartTimeWorker() {
      this.partTimeWorkers.push({
        salary: 0,
        num: 0
      })
    }
  }
}

function padZero(v) {
  if (v < 10) {
    return '0' + v
  } else {
    return v
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.people-num {
  flex: 1;
}

.main-content {
  flex: 2;
}
.level-lavel {
  width: 10vw;
}
.main-content p {
  font-size: 5rem;
}
</style>
