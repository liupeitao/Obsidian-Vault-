# Untitled

```sql
vm.$watch({
  isHot: {
    immediate: true,
    handler(newVal, oldVal) {
      console.log('isHot 发生了变化', newVal, oldVal);
    }
  },
  weather: {
    immediate: true,
    handler(newVal, oldVal) {
      console.log('weather 发生了变化', newVal, oldVal);
    }
  }
});

```
