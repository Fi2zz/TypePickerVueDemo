<template>
  <div class="typepicker-demo">
    <div class="typepicker">
      <div class="prev" @click="onSwitchMonth(-1)">Prev</div>
      <div class="next" @click="onSwitchMonth(1)">Next</div>
      <div v-for="calendar in calendars" :key="calendar.month" class="calendar">
        <div>{{ calendar.year }}年{{ calendar.month }}月</div>
        <ol class="week">
          <li v-for="day in week" :key="day" v-text="day"></li>
        </ol>
        <ol class="dates">
          <li
            v-for="item in calendar.dates"
            :key="item.key"
            :class="item.status"
            @click="onSelect(item)"
            v-text="item.value"
          ></li>
        </ol>
      </div>
    </div>
    <div class="value">{{ selected }}</div>
  </div>
</template>
<script>
import TypePicker from "typepicker";
export default {
  name: "TypePicker",
  data() {
    return {
      calendars: [],
      week: ["日", "一", "二", "三", "四", "五", "六"],
      date: new Date(),
      step: 2,
      selected: []
    };
  },
  created() {
    this.createTypePicker();
  },
  methods: {
    applyInitDates(dates) {
      this.typepicker.apply.dates(dates);
    },

    createTypePicker() {
      this.typepicker = new TypePicker({ size: this.step, selection: 2 });
      this.typepicker.listen(this.typepickerSubscriber);
      this.typepicker.apply.date(this.date);
      this.applyInitDates([
        this.date,
        new Date(this.date.getFullYear(), this.date.getMonth(), 25)
      ]);
    },
    typepickerSubscriber({ type, types, payload }) {
      switch (type) {
        case types.update:
          this.onTypePickerUpdate(payload);
          break;
        case types.select:
          this.onTypePickerSelect(payload);
          break;
      }
    },
    onTypePickerUpdate(payload) {
      this.calendars = payload.map(calendar => {
        calendar.month += 1;
        calendar.dates = calendar.dates.map(item => {
          item.value = item.date.getDate();
          item.key = `${item.date.getMonth()}${item.value}`;
          item.status.isDisabled = item.disabled || item.invalid;
          return item;
        });
        return calendar;
      });
    },
    onTypePickerSelect(payload) {
      this.selected = payload.map(item => item.toLocaleDateString());
    },
    onSwitchMonth(step) {
      const month = this.date.getMonth() + step * this.step;
      this.date.setMonth(month);
      this.typepicker.apply.date(this.date);
    },
    onSelect(item) {
      if (item.invalid || item.disabled) {
        return;
      }
      this.typepicker.apply.select(item.date);
    }
  }
};
</script>

<style lang="less">
.typepicker-demo {
  width: 480px;
  .value {
    font-size: 20px;
    padding: 24px;
  }
}
.typepicker {
  width: 480px;
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  position: relative;
  .prev,
  .next {
    width: 24px;
    height: 24px;
    position: absolute;
    top: 0;
    text-transform: uppercase;
    font-size: 12px;
    cursor: pointer;
    z-index: 999;
  }
  .prev {
    left: 0;
  }
  .next {
    right: 0;
  }

  .calendar {
    width: 240px;
  }
  .dates,
  .week {
    display: flex;
    padding: 0;
    margin: 0;
    list-style: none;

    width: 100%;
    li {
      width: 100/7 * 1%;
    }
  }
  .dates {
    flex-wrap: wrap;

    li {
      cursor: pointer;
    }

    .isDisabled {
      color: #ccc;
    }
    .isActive {
      color: #f51;
    }
    .inRange {
      color: lighten(#f53, 20%);
    }
  }
}
</style>
