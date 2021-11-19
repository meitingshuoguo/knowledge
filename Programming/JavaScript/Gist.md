# 使用moment来构造时间(今天，本周，本月，今年)

```javascript
  setXList = () => {
    const { time, customTimes } = this.state;
    let list = [];
    if (time === 'custom') {
      const daysLength = moment(customTimes[1]).diff(moment(customTimes[0]), 'days');
      list = new Array(daysLength + 1).fill(0);
      list = list.map((l, i) =>
        moment(customTimes[0])
          .add(i, 'day')
          .format('YYYY-MM-DD'),
      );
    }
    if (time === 'day') {
      // 一共多少个小时
      let hourLength = moment().diff(moment().startOf('day'), 'hour') + 1;
      list = new Array(hourLength).fill(0).map((h, i) => `${i < 10 ? '0' + i : i}:00`);
    }
    if (time === 'week') {
      let dayLength = moment().diff(moment().startOf('week'), 'day') + 1;
      list = new Array(dayLength).fill(0).map((h, i) => `星期${this.weekObj[i + 1]}`);
    }
    if (time === 'month') {
      let dayLength = moment().diff(moment().startOf('month'), 'day') + 1;
      list = new Array(dayLength).fill(0).map((h, i) => `${i + 1 < 10 ? '0' + (i + 1) : i + 1}`);
    }
    if (time === 'year') {
      let monthLength = moment().diff(moment().startOf('year'), 'month') + 1;
      list = new Array(monthLength).fill(0).map((h, i) => `${i + 1 < 10 ? '0' + (i + 1) : i + 1}`);
    }
    return list;
  };
```



